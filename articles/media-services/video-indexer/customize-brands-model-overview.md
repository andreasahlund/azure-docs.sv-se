---
title: Anpassa en varumärke modell i Video Indexer – Azure
titleSuffix: Azure Media Services
description: Den här artikeln ger en översikt över vad som är en modell av en varumärkes-modell i Video Indexer och hur du anpassar den.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: kumud
ms.openlocfilehash: e0df698cb431e5e26c69047a9f464904f0546487
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89294245"
---
# <a name="customize-a-brands-model-in-video-indexer"></a>Anpassa en modell med ett varumärke i Video Indexer

Video Indexer stöder varumärkes identifiering från tal-och visuell text vid indexering och Omindexering av video-och ljud innehåll. Funktionen varumärkes identifiering identifierar omnämnanden av produkter, tjänster och företag som föreslås av Bing-databasens varumärken. Om Microsoft till exempel nämns i ett video-eller ljud innehåll eller om det visas i visuell text i en video identifierar Video Indexer det som ett varumärke i innehållet. Varumärken är disambiguated från andra termer som använder kontext.

Varumärkes identifiering är användbart i en mängd olika affärs scenarier, till exempel innehålls Arkiv och identifiering, sammanhangsbaserad annonsering, analys av sociala medier, analys av detalj handeln och många fler. Med Video Indexer varumärkes identifiering kan du indexera varumärkes omnämnanden i tal-och visuell text med hjälp av Bing-databaser och med anpassning genom att skapa en anpassad varumärke modell för varje Video Indexer-konto. Med funktionen för anpassade varumärke modeller kan du välja om Video Indexer ska identifiera varumärken från Bing-databaserna, undanta vissa märken från att identifieras (i princip skapar vi en lista över ej godkända varumärken) och inkluderar varumärken som ska ingå i din modell som kanske inte finns i Bing-databasen (i grunden skapa en lista över godkända varumärken). Den anpassade varumärke modell som du skapar kommer bara att vara tillgänglig i det konto som du skapade modellen i.

## <a name="out-of-the-box-detection-example"></a>Exempel på Box-identifiering

I [Microsoft Build 2017 dag 2](https://www.videoindexer.ai/media/ed6ede78ad/) -presentationen visas varumärket Microsoft Windows flera gånger. Ibland i avskriften, ibland som visuell text och aldrig som orda Grant. Video Indexer identifierar med hög precision som en term är helt beroende av sammanhanget, som täcker över 90k varumärken och ständigt uppdaterar. Vid 02:25 identifierar Video Indexer varumärket från tal och sedan igen vid 02:40 från visuell text, som är en del av Windows-logotypen.

![Översikt över varumärken](./media/content-model-customization/brands-overview.png)

Om du pratar om Windows i samband med bygge identifieras inte ordet "Windows" som ett varumärke, samma för Box, Apple, Fox osv., baserat på avancerade Machine Learning algoritmer som vet hur man disambiguate från kontexten. Varumärkes identifiering fungerar för alla våra språk som stöds. Klicka här för [fullständig Microsoft Build 2017 dag 2-kurs video och index](https://www.videoindexer.ai/media/ed6ede78ad/).

Ta reda på nästa steg om du vill ta med dina egna varumärken.

## <a name="next-steps"></a>Nästa steg

[Anpassa varumärkes-modellen med API: er](customize-brands-model-with-api.md)

[Anpassa varumärkes-modellen med hjälp av webbplatsen](customize-brands-model-with-website.md)
