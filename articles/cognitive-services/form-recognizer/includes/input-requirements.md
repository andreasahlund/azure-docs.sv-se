---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: b3d24ce4dd42ea8122610943379160ca93406c21
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92548071"
---
Formulär tolken arbetar med inmatade dokument som uppfyller följande krav:

* Formatet måste vara JPG, PNG, PDF (text eller scannat) eller TIFF. Text-inbäddade PDF-filer är bäst eftersom det inte går att extrahera fel i tecken extrahering och plats.
* Fil storleken måste vara mindre än 50 MB.
* Bild dimensioner måste vara mellan 50 x 50 pixlar och 10000 x 10000 bild punkter.
* PDF-dimensionerna måste bestå av högst 17 × 17 tum, som motsvarar legal eller a3 pappers storlekar och mindre.
* För PDF och TIFF bearbetas bara de första 200 sidorna (med en prenumeration på kostnads fri nivå, bara de första två sidorna bearbetas).
* Den totala storleken på tränings data uppsättningen måste vara 500 sidor eller mindre.
* Om dina PDF-filer är lösenordsskyddade måste du ta bort låset innan du skickar dem.
* Om du skannar från pappers dokument bör formulär vara av hög kvalitet.
* Texten måste använda det latinska alfabetet (engelska tecken).
* För oövervakad inlärning (utan märkta data) måste data innehålla nycklar och värden.
* För oövervakad inlärning (utan märkta data) måste nycklarna visas ovanför eller till vänster om värdena. de kan inte visas under eller till höger.

Formulär tolken stöder för närvarande inte följande typer av indata:

* Komplexa tabeller (kapslade tabeller, sammanfogade rubriker eller celler osv.).
* Kryss rutor eller alternativ knappar.
