---
title: Data placering för Azure Network Watcher | Microsoft Docs
description: Den här artikeln hjälper dig att förstå data placering för Azure Network Watcher-tjänsten.
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/20/2020
ms.author: damendo
ms.openlocfilehash: 2e6a92a4d08f1603f480a990ad437a90302a8189
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/20/2020
ms.locfileid: "94966096"
---
# <a name="data-residency-for-azure-network-watcher"></a>Data placering för Azure Network Watcher
Med undantag för tjänsten anslutnings övervakare (för hands version), lagrar inte Azure Network Watcher kunddata.


## <a name="connection-monitor-preview-data-residency"></a>Anslutnings övervakare (för hands version) data placering
Tjänsten anslutning övervakare (för hands version) lagrar kund information. Dessa data lagras automatiskt av Network Watcher i en enda region. Anslutnings övervakaren (för hands version) uppfyller automatiskt placering krav för data, inklusive krav som anges i [säkerhets Center](https://azuredatacentermap.azurewebsites.net/).

## <a name="data-residency"></a>Dataplacering
I Azure är funktionen som gör det möjligt att lagra kunddata i en enda region för närvarande endast tillgänglig i Sydostasien region (Singapore) i regionen Asien och stillahavsområdet Geo-och Brasilien södra (Sao Paulo State) i Brasilien geo. För alla andra regioner lagras kund information på Geo. Mer information finns i [säkerhets Center](https://azuredatacentermap.azurewebsites.net/).

## <a name="next-steps"></a>Nästa steg

* Läs en översikt över [Network Watcher](./network-watcher-monitoring-overview.md).