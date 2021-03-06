---
title: 'Självstudie: Distribuera ML-modeller med designern'
titleSuffix: Azure Machine Learning
description: Bygg en förutsägelse analys lösning i Azure Machine Learning designer. Träna, score och distribuera en maskin inlärnings modell med hjälp av dra-och-släpp-moduler.
author: likebupt
ms.author: keli19
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 11/25/2020
ms.custom: designer
ms.openlocfilehash: 28358db2e538867957fcc8d4984cf1c028f9ab2c
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96030382"
---
# <a name="tutorial-deploy-a-machine-learning-model-with-the-designer"></a>Självstudie: Distribuera en maskin inlärnings modell med designern


Du kan distribuera den förutsägande modellen som utvecklats i [del ett av självstudien](tutorial-designer-automobile-price-train-score.md) för att ge andra möjlighet att använda den. I del ett tränar du din modell. Nu är det dags att generera nya förutsägelser baserat på användarindata. I den här delen av självstudien får du göra följande:

> [!div class="checklist"]
> * Skapa en pipeline för real tids härledning.
> * Skapa ett inferencing-kluster.
> * Distribuera real tids slut punkten.
> * Testa real tids slut punkten.

## <a name="prerequisites"></a>Förutsättningar

Slutför [del ett av självstudien](tutorial-designer-automobile-price-train-score.md) och lär dig hur du tränar och poängs ätter en maskin inlärnings modell i designern.

[!INCLUDE [machine-learning-missing-ui](../../includes/machine-learning-missing-ui.md)]

## <a name="create-a-real-time-inference-pipeline"></a>Skapa en pipeline för real tids härledning

Om du vill distribuera din pipeline måste du först konvertera inlärnings pipelinen till en pipeline för real tids härledning. Den här processen tar bort moduler för utbildning och lägger till webb tjänst indata och utdata för att hantera begär Anden.

### <a name="create-a-real-time-inference-pipeline"></a>Skapa en pipeline för real tids härledning

1. Ovanför pipeline-arbetsytan väljer du **skapa en härlednings** pipeline i  >  **real tid**.

    :::image type="content" source="./media/tutorial-designer-automobile-price-deploy/tutorial2-create-inference-pipeline.png"alt-text="Skärm bild som visar var du hittar knappen Skapa pipeline":::

    Din pipeline bör nu se ut så här: 

   ![Skärm bild som visar den förväntade konfigurationen för pipelinen när den har förberedats för distribution](./media/tutorial-designer-automobile-price-deploy/real-time-inference-pipeline.png)

    När du väljer **skapa en härlednings pipeline** inträffar flera saker:
    
    * Den tränade modellen lagras som en **data uppsättnings** modul i modulens palett. Du kan hitta den under **mina data uppsättningar**.
    * Inlärnings moduler som **tränar modell** och **delade data** tas bort.
    * Den sparade utbildade modellen läggs tillbaka i pipelinen.
    * Moduler för **webb tjänst indata** och **webb tjänstens utdata** läggs till. De här modulerna visar var användar data går in i pipelinen och var data returneras.

    > [!NOTE]
    > Som standard förväntas samma data schema som de utbildnings data som används för att skapa den förutsägande pipeline i **webb tjänsten** . I det här scenariot ingår priset i schemat. Priset används dock inte som en faktor under förutsägelsen.
    >

1. Välj **Skicka** och Använd samma beräknings mål och experiment som du använde i del ett.

    Om det här är den första körningen kan det ta upp till 20 minuter innan din pipeline har slutförts. Standard beräknings inställningarna har en minsta Node-storlek på 0, vilket innebär att Designer måste allokera resurser efter inaktivitet. Upprepade pipelines körningar tar mindre tid eftersom beräknings resurserna redan har allokerats. Dessutom använder designern cachelagrade resultat för varje modul för att ytterligare förbättra effektiviteten.

1. Välj **Distribuera**.

## <a name="create-an-inferencing-cluster"></a>Skapa ett inferencing-kluster

I dialog rutan som visas kan du välja från alla befintliga Azure Kubernetes service-kluster (AKS) för att distribuera din modell till. Om du inte har något AKS-kluster kan du använda följande steg för att skapa ett.

1. Välj **beräkning** i dialog rutan som visas för att gå till **beräknings** sidan.

1. I menyfliksområdet navigering väljer du **härlednings kluster**  >  **+ ny**.

    ![Skärm bild som visar hur du kommer till fönstret nytt fönster för utgångs kluster](./media/tutorial-designer-automobile-price-deploy/new-inference-cluster.png)
   
1. Konfigurera en ny Kubernetes-tjänst i fönstret fönster för utlåsnings kluster.

1. Ange *AKS-Compute* för **beräknings namnet**.
    
1. Välj en närliggande region som är tillgänglig för **regionen**.

1. Välj **Skapa**.

    > [!NOTE]
    > Det tar cirka 15 minuter att skapa en ny AKS-tjänst. Du kan kontrol lera etablerings statusen på sidan för att lösa **kluster** .
    >

## <a name="deploy-the-real-time-endpoint"></a>Distribuera real tids slut punkten

När din AKS-tjänst har slutfört etableringen återgår du till inferencing-pipeline i real tid för att slutföra distributionen.

1. Välj **distribuera** ovanför arbets ytan.

1. Välj **distribuera ny slut punkt i real tid**. 

1. Välj det AKS-kluster som du skapade.

1. Välj **Distribuera**.
    
    :::image type="content" source="./media/tutorial-designer-automobile-price-deploy/setup-endpoint.png"alt-text="Skärm bild som visar hur du konfigurerar en ny slut punkt för Real tid":::

    Ett meddelande visas ovanför arbets ytan när distributionen är klar. Det kan ta några minuter.

## <a name="view-the-real-time-endpoint"></a>Visa real tids slut punkten

När distributionen är klar kan du Visa real tids slut punkten genom att gå till sidan **slut punkter** .

1. På sidan **slut punkter** väljer du den slut punkt som du har distribuerat.

1. På fliken **information** kan du se mer information, till exempel rest-URI, status och taggar.

1. På fliken **förbrukare** kan du hitta säkerhets nycklar och ange autentiseringsmetoder.

1. På fliken **distributions loggar** hittar du detaljerade distributions loggar för din real tids slut punkt. 

Mer information om hur du konsumerar din webb tjänst finns i [använda en modell som distribueras som en webb tjänst](how-to-consume-web-service.md)

## <a name="clean-up-resources"></a>Rensa resurser

[!INCLUDE [aml-ui-cleanup](../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Nästa steg

I den här självstudien har du lärt dig viktiga steg i hur du skapar, distribuerar och använder en maskin inlärnings modell i designern. Mer information om hur du kan använda designern för att lösa andra typer av problem finns i våra andra exempel pipelines.

> [!div class="nextstepaction"]
> [Designer-exempel](samples-designer.md)
