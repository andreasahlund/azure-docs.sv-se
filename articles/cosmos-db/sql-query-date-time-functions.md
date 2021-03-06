---
title: Datum-och tids funktioner i Azure Cosmos DB frågespråk
description: Lär dig mer om datum-och tids funktioner i SQL system-funktioner i Azure Cosmos DB att utföra DateTime-och timestamp-åtgärder.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 08/18/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: d8b6e932f4cd5f4119d93f3fed7db524f65cbc1f
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93338959"
---
# <a name="date-and-time-functions-azure-cosmos-db"></a>Datum-och tids funktioner (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Med funktionen för datum-och tids funktioner kan du utföra DateTime-och timestamp-åtgärder i Azure Cosmos DB.

## <a name="functions-to-obtain-the-date-and-time"></a>Funktioner för att hämta datum och tid

Med följande skalära funktioner kan du hämta det aktuella UTC-datumet och-tiden i tre former: en sträng som följer ISO 8601-formatet, en numerisk tidstämpel vars värde är antalet millisekunder som har förflutit sedan UNIX-epoken eller numeriska Tick vars värde är antalet 100 nanosekunder-Tick som har förflutit sedan UNIX-epoken :

* [GetCurrentDateTime](sql-query-getcurrentdatetime.md)
* [GetCurrentTimestamp](sql-query-getcurrenttimestamp.md)
* [GetCurrentTicks](sql-query-getcurrentticks.md)

## <a name="functions-to-work-with-datetime-values"></a>Funktioner för att arbeta med DateTime-värden

Med följande funktioner kan du enkelt ändra datum-, tidsstämpel-och skal värden:

* [DateTimeAdd](sql-query-datetimeadd.md)
* [DateTimeDiff](sql-query-datetimediff.md)
* [DateTimeFromParts](sql-query-datetimefromparts.md)
* [DateTimePart](sql-query-datetimepart.md)
* [DateTimeToTicks](sql-query-datetimetoticks.md)
* [DateTimeToTimestamp](sql-query-datetimetotimestamp.md)
* [TicksToDateTime](sql-query-tickstodatetime.md)
* [TimestampToDateTime](sql-query-timestamptodatetime.md)

## <a name="next-steps"></a>Nästa steg

- [System funktioner Azure Cosmos DB](sql-query-system-functions.md)
- [Introduktion till Azure Cosmos DB](introduction.md)
- [Användardefinierade funktioner](sql-query-udfs.md)
- [Aggregeringar](sql-query-aggregates.md)
