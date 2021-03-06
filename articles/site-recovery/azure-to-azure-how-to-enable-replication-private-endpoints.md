---
title: Aktivera replikering för privata slut punkter i Azure Site Recovery
description: Den här artikeln beskriver hur du konfigurerar replikering för virtuella datorer med privata slut punkter från en Azure-region till en annan med hjälp av Site Recovery.
author: Harsha-CS
ms.author: harshacs
ms.service: site-recovery
ms.topic: article
ms.date: 07/14/2020
ms.custom: references_regions
ms.openlocfilehash: 2db8d09511385039ea79ad1ed1f50177156f69c8
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92369938"
---
# <a name="replicate-machines-with-private-endpoints"></a>Replikera datorer med privata slut punkter

Med Azure Site Recovery kan du använda privata [Azure privat länk](../private-link/private-endpoint-overview.md) -slutpunkter för att replikera dina datorer inifrån ett isolerat virtuellt nätverk. Åtkomst till privata slut punkter till ett återställnings valv stöds i alla Azures regioner med kommersiell & myndigheter.

Den här artikeln innehåller anvisningar för att utföra följande steg:

- Skapa ett Azure Backup Recovery Services valv för att skydda dina datorer.
- Aktivera en hanterad identitet för valvet och ge de behörigheter som krävs för att få åtkomst till kundens lagrings konton för att replikera trafik från källa till mål platser. Hanterad identitets åtkomst för lagring krävs när du konfigurerar privat länk åtkomst till valvet.
- Gör DNS-ändringar nödvändiga för privata slut punkter
- Skapa och godkänn privata slut punkter för ett valv i ett virtuellt nätverk
- Skapa privata slut punkter för lagrings kontona. Du kan fortsätta att tillåta offentlig eller brand Väggs åtkomst för lagring vid behov. Det är inte obligatoriskt att skapa en privat slut punkt för åtkomst till lagrings utrymme för Azure Site Recovery.
  
Nedan visas en referens arkitektur för hur arbets flödet för replikering ändras med privata slut punkter.

:::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/architecture.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter.":::

## <a name="prerequisites-and-caveats"></a>Krav och varningar

- Privata slut punkter kan bara skapas för nya Recovery Services-valv som inte har några objekt registrerade för valvet. Därför måste privata slut punkter **skapas innan objekt läggs till i valvet**. Granska pris strukturen för [privata slut punkter](https://azure.microsoft.com/pricing/details/private-link/).
- När en privat slut punkt skapas för ett valv är valvet låst och **kan inte nås från andra nätverk än de nätverk som har privata slut punkter**.
- Azure Active Directory stöder för närvarande inte privata slut punkter. Därför måste IP-adresser och fullständigt kvalificerade domän namn som krävs för att Azure Active Directory ska fungera i en region tillåtas utgående åtkomst från det skyddade nätverket. Du kan också använda nätverks säkerhets grupp tag gen "Azure Active Directory" och Azure Firewall-taggar för att tillåta åtkomst till Azure Active Directory, efter vad som är tillämpligt.
- **Minst sju IP-adresser krävs** i under näten för både käll datorerna och återställnings datorerna. När du skapar en privat slut punkt för valvet skapar Site Recovery fem privata Länkar för åtkomst till dess mikrotjänster. När du aktiverar replikeringen läggs dessutom två ytterligare privata länkar till i käll-och mål områdes paret.
- **En ytterligare IP-adress krävs** i både käll-och återställnings under nätet. Den här IP-adressen behövs bara när du behöver använda privata slut punkter för att ansluta till cache Storage-konton.
  Det går bara att skapa privata slut punkter för lagring på Generell användning v2-typ. Granska pris strukturen för [data överföring på GPv2](https://azure.microsoft.com/pricing/details/storage/page-blobs/).

 ## <a name="creating-and-using-private-endpoints-for-site-recovery"></a>Skapa och använda privata slut punkter för Site Recovery

 Det här avsnittet beskriver de steg som ingår i att skapa och använda privata slut punkter för Azure Site Recovery i dina virtuella nätverk.

> [!NOTE]
> Vi rekommenderar starkt att du följer de här stegen i samma ordning som anges. Om du inte gör det kan det leda till att valvet återges att det inte går att använda privata slut punkter och kräver att du startar om processen med ett nytt valv.

## <a name="create-a-recovery-services-vault"></a>skapar ett Recovery Services-valv

Ett Recovery Services-valv är en entitet som innehåller replikeringsinformation för datorer och används för att utlösa Site Recovery åtgärder. Mer information finns i [skapa ett Recovery Services-valv](./azure-to-azure-tutorial-enable-replication.md#create-a-recovery-services-vault).

## <a name="enable-the-managed-identity-for-the-vault"></a>Aktivera den hanterade identiteten för valvet.

Med en [hanterad identitet](../active-directory/managed-identities-azure-resources/overview.md) kan valvet få åtkomst till kundens lagrings konton. Site Recovery behöver åtkomst till käll lagring, mål lagring och cache/logg lagrings konton beroende på scenario kravet.
Hanterad identitets åtkomst är nödvändig när du använder privata länk tjänster för valvet.

1. Gå till Recovery Services-valvet. Välj **identitet** under _Inställningar_.

   :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/enable-managed-identity-in-vault.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter.":::

1. Ändra **statusen** till _på_ och välj **Spara**.

1. Ett **objekt-ID** genereras som anger att valvet nu har registrerats med Azure Active Directory.

## <a name="create-private-endpoints-for-the-recovery-services-vault"></a>Skapa privata slut punkter för Recovery Services valvet

Om du vill aktivera både redundans och återställning efter fel för virtuella Azure-datorer behöver du två privata slut punkter för valvet. En privat slut punkt för att skydda datorer i käll nätverket och en annan för återskydd av misslyckade datorer i återställnings nätverket.

Se till att du skapar ett virtuellt återställnings nätverk i mål regionen och under den här installations processen.

Skapa den första privata slut punkten för valvet i ditt virtuella käll nätverk med hjälp av det privata länk centret i portalen eller via [Azure PowerShell](../private-link/create-private-endpoint-powershell.md). Skapa den andra privata slut punkten för valvet i återställnings nätverket. Nedan följer stegen för att skapa den privata slut punkten i käll nätverket. Upprepa samma vägledning för att skapa den andra privata slut punkten.

1. Sök efter och välj "privat länk" i Sök fältet Azure Portal. Den här åtgärden tar dig till det privata länk centret.

   :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/search-private-links.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter.":::

1. I det vänstra navigerings fältet väljer du **privata slut punkter**. På sidan privata slut punkter väljer du ** \+ Lägg till** för att börja skapa en privat slut punkt för valvet.

   :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/create-private-endpoints.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter." måste du ange information om hur du skapar din privata slut punkts anslutning.

   1. **Grundläggande**information: Fyll i den grundläggande informationen för dina privata slut punkter. Regionen ska vara samma som käll datorerna.

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/create-private-endpoints-basic-tab.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter.":::

   1. **Resurs**: den här fliken kräver att du nämner den plattform-som-tjänst-resurs som du vill skapa anslutningen för. Välj _Microsoft. RecoveryServices/valv_ från **resurs typen** för den valda prenumerationen. Välj sedan namnet på Recovery Services valvet för **resurs** och ange _Azure Site Recovery_ som **mål under resurs**.

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/create-private-endpoints-resource-tab.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter.":::

   1. **Konfiguration**: i konfiguration anger du det virtuella nätverk och undernät där du vill att den privata slut punkten ska skapas. Det här virtuella nätverket är det nätverk där den virtuella datorn finns. Aktivera integrering med privat DNS-zon genom att välja **Ja**. Välj en DNS-zon som redan har skapats eller skapa en ny. Genom att välja **Ja** länkar du automatiskt zonen till det virtuella käll nätverket och lägger till de DNS-poster som krävs för DNS-matchning av nya IP-adresser och fullständigt kvalificerade domän namn som skapats för den privata slut punkten.

      Se till att du väljer att skapa en ny DNS-zon för varje ny privat slut punkt som ansluter till samma valv. Om du väljer en befintlig privat DNS-zon skrivs de tidigare CNAME-posterna över. Se [vägledning för privat slut punkt](../private-link/private-endpoint-overview.md#private-endpoint-properties) innan du fortsätter.

      Om din miljö har en nav-och eker-modell behöver du bara en privat slut punkt och bara en privat DNS-zon för hela installationen eftersom alla dina virtuella nätverk redan har peering aktiverat mellan dem. Mer information finns i [DNS-integrering för privata slut punkter](../private-link/private-endpoint-dns.md#virtual-network-workloads-without-custom-dns-server).

      Om du vill skapa en privat DNS-zon manuellt följer du stegen i [skapa privata DNS-zoner och lägger till DNS-poster manuellt](#create-private-dns-zones-and-add-dns-records-manually).

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/create-private-endpoints-configuration-tab.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter.":::

   1. **Taggar**: om du vill kan du lägga till taggar för din privata slut punkt.

   1. **Granska \+ skapa**: När verifieringen är klar väljer du **skapa** för att skapa den privata slut punkten.

När den privata slut punkten har skapats läggs fem fullständigt kvalificerade domän namn till i den privata slut punkten. Dessa länkar gör att datorerna i det virtuella nätverket får åtkomst till alla nödvändiga Site Recovery mikrotjänster i kontexten för valvet. Senare, när du aktiverar replikeringen, läggs två ytterligare fullständigt kvalificerade domän namn till i samma privata slut punkt.

De fem domän namnen är formaterade med följande mönster:

`{Vault-ID}-asr-pod01-{type}-.{target-geo-code}.siterecovery.windowsazure.com`

## <a name="approve-private-endpoints-for-site-recovery"></a>Godkänn privata slut punkter för Site Recovery

Om användaren som skapar den privata slut punkten också är ägare av Recovery Services-valvet, godkänns den privata slut punkten som skapades ovan automatiskt inom några minuter. Annars måste ägaren av valvet godkänna den privata slut punkten innan du kan använda den. Om du vill godkänna eller avvisa en begärd privat slut punkts anslutning går du till **anslutningar för privata slut punkter** under "Inställningar" på sidan återställnings valv.

Du kan gå till den privata slut punkts resursen för att granska status för anslutningen innan du fortsätter.

:::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/vault-private-endpoint-connections.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter.":::

## <a name="optional-create-private-endpoints-for-the-cache-storage-account"></a><a name="create-private-endpoints-for-the-cache-storage-account"></a>Valfritt Skapa privata slut punkter för lagrings kontot för cache

En privat slut punkt till Azure Storage kan användas. Att skapa privata slut punkter för lagrings åtkomst är _valfritt_ för Azure Site Recovery replikering. När du skapar en privat slut punkt för lagring, gäller följande krav:

- Du behöver en privat slut punkt för cache-/logg lagrings kontot i det virtuella käll nätverket.
- Du behöver en andra privat slut punkt när du återskapar de misslyckade datorerna i återställnings nätverket. Den här privata slut punkten är för det nya lagrings kontot som skapas i mål regionen.

> [!NOTE]
> Det går bara att skapa en privat slut punkt för lagring på ett **generell användning v2** -lagrings konton. För pris information, se [standard Page BLOB-priser](https://azure.microsoft.com/pricing/details/storage/page-blobs/).

Följ [rikt linjerna för att skapa privat lagring](../private-link/tutorial-private-endpoint-storage-portal.md#create-storage-account-with-a-private-endpoint) för att skapa ett lagrings konto med privat slut punkt. Se till att välja **Ja** för integrering med privat DNS-zon. Välj en DNS-zon som redan har skapats eller skapa en ny.

## <a name="grant-required-permissions-to-the-vault"></a>Bevilja de behörigheter som krävs för valvet

Om dina virtuella datorer använder hanterade diskar, behöver du bara ge hanterade identitets behörigheter till cache Storage-kontona. Om de virtuella datorerna använder ohanterade diskar måste du bevilja behörigheter för hanterad identitet för käll-, cache-och mål lagrings konton. I så fall måste du skapa mål lagrings kontot i förväg.

Innan du aktiverar replikering av virtuella datorer måste den hanterade identiteten för valvet ha följande roll behörigheter beroende på typ av lagrings konto:

- Resource Manager-baserade lagrings konton (standard typ):
  - [Deltagare](../role-based-access-control/built-in-roles.md#contributor)
  - [Storage Blob Data-deltagare](../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)
- Resource Manager-baserade lagrings konton (Premium typ):
  - [Deltagare](../role-based-access-control/built-in-roles.md#contributor)
  - [Storage Blob Data-ägare](../role-based-access-control/built-in-roles.md#storage-blob-data-owner)
- Klassiska lagrings konton:
  - [Klassisk lagrings konto deltagare](../role-based-access-control/built-in-roles.md#classic-storage-account-contributor)
  - [Klassisk lagrings kontots nyckel operatörs tjänst roll](../role-based-access-control/built-in-roles.md#classic-storage-account-key-operator-service-role)

Följande steg beskriver hur du lägger till en roll tilldelning till dina lagrings konton, en i taget:

1. Gå till lagrings kontot och navigera till **åtkomst kontroll (IAM)** till vänster på sidan.

1. När du har aktiverat **åtkomst kontroll (IAM)** i rutan Lägg till en roll tilldelning väljer du **Lägg till**.

   :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/storage-role-assignment.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter.":::

1. På sidan Lägg till en roll tilldelning väljer du rollen i listan ovan i list rutan **roll** . Ange **namnet** på valvet och välj **Spara**.

   :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/storage-role-assignment-select-role.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter." och markera kryss rutan Tillåt att betrodda Microsoft-tjänster har åtkomst till det här lagrings kontot i **undantag**.

## <a name="protect-your-virtual-machines"></a>Skydda dina virtuella datorer

När alla konfigurationer ovan har slutförts fortsätter du med att aktivera replikering för dina virtuella datorer. Alla Site Recovery åtgärder fungerar utan ytterligare steg om DNS-integrering användes när privata slut punkter skapades i valvet. Men om DNS-zonerna skapas och konfigureras manuellt behöver du ytterligare steg för att lägga till specifika DNS-poster i både käll-och mål-DNS-zoner när du har aktiverat replikeringen. Mer information och anvisningar finns i [skapa privata DNS-zoner och lägga till DNS-poster manuellt](#create-private-dns-zones-and-add-dns-records-manually).

## <a name="create-private-dns-zones-and-add-dns-records-manually"></a>Skapa privata DNS-zoner och Lägg till DNS-poster manuellt

Om du inte har valt alternativet att integrera med en privat DNS-zon när du skapade privat slut punkt för valvet följer du stegen i det här avsnittet.

Skapa en privat DNS-zon så att mobilitets agenten kan matcha privata länkar fullständigt kvalificerade domän namn till privata IP-adresser.

1. Skapa en privat DNS-zon

   1. Sök efter "Privat DNS zon" i Sök fältet **alla tjänster** och välj "privat DNS zoner" i list rutan.

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/search-private-dns-zone.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter.":::

   1. På sidan Privat DNS zoner väljer du knappen ** \+ Lägg till** för att börja skapa en ny zon.

   1. Fyll i den information som krävs på sidan Skapa privat DNS-zon. Ange namnet på den privata DNS-zonen som `privatelink.siterecovery.windowsazure.com` . Du kan välja vilken resurs grupp och vilken prenumeration som helst för att skapa den.

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/create-private-dns-zone.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter.":::

   1. Fortsätt till fliken **Granska \+ skapande** för att granska och skapa DNS-zonen.

1. Länka en privat DNS-zon till ditt virtuella nätverk

   De privata DNS-zoner som skapats ovan måste nu länkas till det virtuella nätverk där dina servrar finns. Du måste också länka den privata DNS-zonen till det virtuella mål nätverket i förväg.

   1. Gå till den privata DNS-zon som du skapade i föregående steg och navigera till **virtuella nätverks länkar** till vänster på sidan. Sedan väljer du knappen ** \+ Lägg till** .

   1. Fyll i den information som krävs. Fälten **prenumeration** och **virtuell nätverk** måste fyllas i med motsvarande information om det virtuella nätverk där dina servrar finns. De andra fälten måste vara kvar som de är.

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/add-virtual-network-link.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter." i **Översikt**. Som du ser i exemplet nedan läggs det första fullständigt kvalificerade domän namnet från den privata slut punkten till i post uppsättningen i den privata DNS-zonen.

      Dessa fullständigt kvalificerade domän namn matchar mönstret: `{Vault-ID}-asr-pod01-{type}-.{target-geo-code}.siterecovery.windowsazure.com`

      :::image type="content" source="./media/azure-to-azure-how-to-enable-replication-private-endpoints/add-record-set.png" alt-text="Referens arkitektur för Site Recovery med privata slut punkter.":::

   > [!NOTE]
   > När du har aktiverat replikering skapas två fullständiga domän namn på de privata slut punkterna i båda regionerna. Se till att du lägger till DNS-posterna för de nya fullständigt kvalificerade domän namnen också.

## <a name="next-steps"></a>Nästa steg

Nu när du har aktiverat privata slut punkter för replikeringen av virtuella datorer kan du se följande sidor för ytterligare och relaterad information:

- [Replikera virtuella Azure-datorer till en annan Azure-region](./azure-to-azure-how-to-enable-replication.md)
- [Självstudie: Konfigurera haveri beredskap för virtuella Azure-datorer](./azure-to-azure-tutorial-enable-replication.md)