---
title: Läs in data i Azure Storage miljöer – team data science process
description: Lär dig mer om hur du matar in data i olika mål miljöer där data lagras och bearbetas.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: f8eab59d810fb825dbebf80d01d8efd2dd0a9841
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "76720545"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Läs in data i lagringsmiljöer för analys

Team data science-processen kräver att data matas in eller läses in på det lämpligaste sättet i varje steg. Data destinationer kan omfatta Azure Blob Storage, SQL Azure-databaser, SQL Server på Azure VM, HDInsight (Hadoop), Synapse Analytics och Azure Machine Learning. 

I följande artiklar beskrivs hur du matar in data i olika mål miljöer där data lagras och bearbetas.

* Till/från [Azure Blob Storage](move-azure-blob.md)
* För att [SQL Server på virtuell Azure-dator](move-sql-server-virtual-machine.md)
* För att [Azure SQL Database](move-sql-azure.md)
* Till [Hive-tabeller](move-hive-tables.md)
* Till [partitionerade SQL-tabeller](parallel-load-sql-partitioned-tables.md)
* Från [lokala SQL Server](move-sql-azure-adf.md)

De tekniska och affärs behoven, samt den ursprungliga platsen, formatet och storleken på dina data avgör den bästa data inmatnings planen. Det är inte ovanligt att det finns flera steg i en bra plan. Den här aktivitetssekvensen kan omfatta exempelvis data utforskning, för bearbetning, rensning, nedsampling och modell träning.  Azure Data Factory är en rekommenderad Azure-resurs för att dirigera data förflyttning och omvandling.
