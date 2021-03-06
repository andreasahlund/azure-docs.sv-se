---
title: 'Självstudie: Azure Active Directory integrering med Lucidchart | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Lucidchart.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.openlocfilehash: 3216718f076140cc9e7a5dec9048ee04adc5d96b
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92458384"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-lucidchart"></a>Självstudie: Azure Active Directory integration med enkel inloggning (SSO) med Lucidchart

I den här självstudien får du lära dig hur du integrerar Lucidchart med Azure Active Directory (Azure AD). När du integrerar Lucidchart med Azure AD kan du:

* Kontroll i Azure AD som har åtkomst till Lucidchart.
* Gör det möjligt för användarna att logga in automatiskt till Lucidchart med sina Azure AD-konton.
* Hantera dina konton på en central plats – Azure Portal.

Mer information om SaaS app integration med Azure AD finns i [Vad är program åtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Krav

För att komma igång behöver du följande objekt:

* En Azure AD-prenumeration. Om du inte har någon prenumeration kan du få ett [kostnads fritt konto](https://azure.microsoft.com/free/).
* Lucidchart för enkel inloggning (SSO) aktive rad.

## <a name="scenario-description"></a>Scenariobeskrivning

I den här självstudien konfigurerar och testar du Azure AD SSO i en test miljö.

* Lucidchart stöder **SP** -INITIERAd SSO
* Lucidchart stöder **just-in-Time** User-etablering
* När du har konfigurerat Lucidchart kan du framtvinga sessionsnycklar som skyddar exfiltrering och intrånget för organisationens känsliga data i real tid. Sessions kontroller utökas från villkorlig åtkomst. [Lär dig hur du tvingar fram sessions kontroll med Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad)

## <a name="adding-lucidchart-from-the-gallery"></a>Lägga till Lucidchart från galleriet

Om du vill konfigurera integreringen av Lucidchart i Azure AD måste du lägga till Lucidchart från galleriet i listan över hanterade SaaS-appar.

1. Logga in på [Azure Portal](https://portal.azure.com) med antingen ett arbets-eller skol konto eller en personlig Microsoft-konto.
1. I det vänstra navigerings fönstret väljer du tjänsten **Azure Active Directory** .
1. Navigera till **företags program** och välj sedan **alla program**.
1. Välj **nytt program**om du vill lägga till ett nytt program.
1. I avsnittet **Lägg till från galleriet** , skriver du **Lucidchart** i sökrutan.
1. Välj **Lucidchart** från resultat panelen och Lägg sedan till appen. Vänta några sekunder medan appen läggs till i din klient organisation.


## <a name="configure-and-test-azure-ad-single-sign-on-for-lucidchart"></a>Konfigurera och testa enkel inloggning med Azure AD för Lucidchart

Konfigurera och testa Azure AD SSO med Lucidchart med hjälp av en test användare som heter **B. Simon**. För att SSO ska fungera måste du upprätta en länk relation mellan en Azure AD-användare och den relaterade användaren i Lucidchart.

Om du vill konfigurera och testa Azure AD SSO med Lucidchart, slutför du följande Bygg stenar:

1. **[Konfigurera Azure AD SSO](#configure-azure-ad-sso)** – så att användarna kan använda den här funktionen.
    * **[Skapa en Azure AD-test](#create-an-azure-ad-test-user)** för att testa enkel inloggning med Azure AD med B. Simon.
    * **[Tilldela Azure AD-testuser](#assign-the-azure-ad-test-user)** -för att aktivera B. Simon för att använda enkel inloggning med Azure AD.
1. **[Konfigurera LUCIDCHART SSO](#configure-lucidchart-sso)** – för att konfigurera inställningarna för enkel inloggning på program sidan.
    * **[Skapa Lucidchart test User](#create-lucidchart-test-user)** -om du vill ha en motsvarighet till B. Simon i Lucidchart som är länkad till Azure AD-representation av användare.
1. **[Testa SSO](#test-sso)** – för att kontrol lera om konfigurationen fungerar.

## <a name="configure-azure-ad-sso"></a>Konfigurera Azure AD SSO

Följ de här stegen för att aktivera Azure AD SSO i Azure Portal.

1. I [Azure Portal](https://portal.azure.com/)går du till sidan för program integrering i **Lucidchart** , letar upp avsnittet **Hantera** och väljer **enkel inloggning**.
1. På sidan **Välj metod för enkel inloggning** väljer du **SAML**.
1. På sidan **Konfigurera enkel inloggning med SAML** klickar du på ikonen Redigera/penna för **grundläggande SAML-konfiguration** för att redigera inställningarna.

   ![Redigera grundläggande SAML-konfiguration](common/edit-urls.png)

1. I avsnittet **grundläggande SAML-konfiguration** anger du värden för följande fält:

   I text rutan **inloggnings-URL** anger du en URL som:  `https://chart2.office.lucidchart.com/saml/sso/azure`

5. På sidan **Set up Single Sign-On with SAML** (Konfigurera enkel inloggning med SAML) går du till avsnittet **SAML Signing Certificate** (SAML-signeringscertifikat), klickar på **Ladda ned** för att ladda ned **Federation Metadata-XML** från de angivna alternativen enligt dina behov och spara den på datorn.

    ![Länk för nedladdning av certifikatet](common/metadataxml.png)

6. I avsnittet **Konfigurera Lucidchart** kopierar du lämpliga URL: er enligt ditt krav.

    ![Kopiera konfigurations-URL:er](common/copy-configuration-urls.png)

    a. Inloggnings-URL

    b. Azure AD-identifierare

    c. Utloggnings-URL

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

I det här avsnittet ska du skapa en test användare i Azure Portal som kallas B. Simon.

1. I den vänstra rutan i Azure Portal väljer du **Azure Active Directory**, väljer **användare**och väljer sedan **alla användare**.
1. Välj **ny användare** överst på skärmen.
1. I **användar** egenskaperna följer du de här stegen:
   1. I **Namn**-fältet skriver du `B.Simon`.  
   1. I fältet **användar namn** anger du username@companydomain.extension . Till exempel `B.Simon@contoso.com`.
   1. Markera kryssrutan **Visa lösenord** och skriv sedan ned det värde som visas i rutan **Lösenord**.
   1. Klicka på **Skapa**.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändaren

I det här avsnittet ska du aktivera B. Simon för att använda enkel inloggning med Azure genom att bevilja åtkomst till Lucidchart.

1. I Azure Portal väljer du **företags program**och väljer sedan **alla program**.
1. I listan program väljer du **Lucidchart**.
1. På sidan Översikt för appen letar du reda på avsnittet **Hantera** och väljer **användare och grupper**.

   ![Länken ”Användare och grupper”](common/users-groups-blade.png)

1. Välj **Lägg till användare**och välj sedan **användare och grupper** i dialog rutan **Lägg till tilldelning** .

    ![Länken Lägg till användare](common/add-assign-user.png)

1. I dialog rutan **användare och grupper** väljer du **B. Simon** från listan användare och klickar sedan på knappen **Välj** längst ned på skärmen.
1. Om du förväntar dig ett roll värde i SAML Assertion, i dialog rutan **Välj roll** , väljer du lämplig roll för användaren i listan och klickar sedan på knappen **Välj** längst ned på skärmen.
1. Klicka på knappen **tilldela** i dialog rutan **Lägg till tilldelning** .

## <a name="configure-lucidchart-sso"></a>Konfigurera Lucidchart SSO

1. Logga in på din Lucidchart-företags webbplats som administratör i ett annat webbläsarfönster.

2. I menyn högst upp klickar du på **team**.

    ![Lag](./media/lucidchart-tutorial/ic791190.png "Team")

3. Klicka på **program \> Hantera SAML**.

    ![Hantera SAML](./media/lucidchart-tutorial/ic791191.png "Hantera SAML")

4. Utför följande steg på dialog sidan **Inställningar för SAML-autentisering** :

    a. Välj **Aktivera SAML-autentisering**och klicka sedan på **valfri**.

    ![Inställningar för SAML-autentisering](./media/lucidchart-tutorial/ic791192.png "Inställningar för SAML-autentisering")

    b. Skriv din domän i text rutan **domän** och klicka sedan på **ändra certifikat**.

    ![Ändra certifikat](./media/lucidchart-tutorial/ic791193.png "Ändra certifikat")

    c. Öppna den hämtade metadatafilen, kopiera innehållet och klistra in den i text rutan **Ladda upp metadata** .

    ![Ladda upp metadata](./media/lucidchart-tutorial/ic791194.png "Ladda upp metadata")

    d. Välj **Lägg automatiskt till nya användare i teamet**och klicka sedan på **Spara ändringar**.

    ![Spara ändringar](./media/lucidchart-tutorial/ic791195.png "Spara ändringar")

### <a name="create-lucidchart-test-user"></a>Skapa Lucidchart test användare

Det finns inget åtgärds objekt som du kan använda för att konfigurera användar etablering till Lucidchart.  När en tilldelad användare försöker logga in på Lucidchart med hjälp av åtkomst panelen kontrollerar Lucidchart om användaren finns.  

Om det inte finns något användar konto tillgängligt ännu, skapas det automatiskt av Lucidchart.

## <a name="test-sso"></a>Testa SSO 

I det här avsnittet testar du konfigurationen för enkel inloggning Azure AD med hjälp av åtkomstpanelen.

När du klickar på panelen Lucidchart på åtkomst panelen, bör du loggas in automatiskt på den Lucidchart som du ställer in SSO för. Mer information om åtkomstpanelen finns i [introduktionen till åtkomstpanelen](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ytterligare resurser

- [ Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory ](./tutorial-list.md)

- [Vad är program åtkomst och enkel inloggning med Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [Vad är villkorlig åtkomst i Azure Active Directory?](../conditional-access/overview.md)

- [Prova Lucidchart med Azure AD](https://aad.portal.azure.com/)

- [Vad är session Control i Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)

- [Så här skyddar du Lucidchart med avancerad synlighet och kontroller](/cloud-app-security/proxy-intro-aad)