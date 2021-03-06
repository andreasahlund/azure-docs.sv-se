---
title: Flytta Azure Network Watcher-resurser | Microsoft Docs
description: Flytta Azure Network Watcher resurser mellan regioner
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
ms.date: 07/27/2020
ms.author: damendo
ms.openlocfilehash: 84764e73ec5b4ada8c204147def310326a3c7bdd
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/20/2020
ms.locfileid: "94948433"
---
# <a name="moving-azure-network-watcher-resources-across-regions"></a>Flytta Azure Network Watcher resurser mellan regioner

## <a name="moving-the-network-watcher-resource"></a>Flytta Network Watcher resursen
Network Watcher resursen representerar backend-tjänsten för Network Watcher och hanteras helt av Azure. Kunderna behöver inte hantera den. Flytt åtgärden stöds inte på den här resursen.

## <a name="moving-child-resources-of-network-watcher"></a>Flytta underordnade resurser för Network Watcher
Det finns för närvarande inte stöd för att flytta resurser mellan regioner för en underordnad resurs av `*networkWatcher*` resurs typen.

## <a name="next-steps"></a>Nästa steg
* Läs [Network Watcher översikt](./network-watcher-monitoring-overview.md)
* Se [vanliga frågor och svar om Network Watcher](./frequently-asked-questions.md)