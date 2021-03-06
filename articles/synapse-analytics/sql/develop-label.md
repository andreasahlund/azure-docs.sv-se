---
title: Använda fråge etiketter i Synapse SQL
description: I den här artikeln finns några viktiga tips för att använda fråge etiketter i Synapse SQL.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 0b2c03c5c7ea8c65cb1cde3cbdb73b6bb838dc06
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93324460"
---
# <a name="use-query-labels-in-synapse-sql"></a>Använda fråge etiketter i Synapse SQL

I den här artikeln finns några viktiga tips för att använda fråge etiketter i Synapse SQL.

> [!NOTE]
> SQL-poolen utan server (för hands version) stöder inte märknings frågor.

## <a name="what-are-query-labels"></a>Vad är fråge etiketter

Dedikerad SQL-pool stöder ett begrepp som kallas fråge etiketter. Låt oss titta på ett exempel innan du går igenom ett djup:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Den sista raden Taggar strängen "min fråga etikett" till frågan. Den här taggen är användbar eftersom etiketten är frågekörning genom DMV: er. Fråga efter etiketter är en mekanism för att hitta problem frågor och hjälpa till att identifiera förloppet genom en ELT körning.

Bra namn konventioner är mest användbara. Exempel: om du startar etiketten med PROJECT, PROCEDURe, STATEMENT eller COMMENT identifieras frågan bland all kod i käll kontrollen.

Följande fråga använder en vy med dynamisk hantering för att söka efter etikett:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Det är viktigt att sätta hakparenteser eller dubbla citat tecken runt ordet etikett vid frågor. Label är ett reserverat ord och orsakar ett fel när det inte är avgränsat. 
> 
> 

## <a name="next-steps"></a>Nästa steg
Mer utvecklings tips finns i [utvecklings översikt](develop-overview.md).


