---
title: ta med fil
description: ta med fil
services: storage
author: twooley
ms.service: storage
ms.topic: include
ms.date: 09/30/2020
ms.author: twooley
ms.custom: include file
ms.openlocfilehash: 297641bbaccb44739d67fdd26f0c1f64062bba46
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/24/2020
ms.locfileid: "95563257"
---
**Azure Data Lake Storage Gen2** är inte en dedikerad tjänst eller en typ av lagrings konto. Det är den senaste versionen av funktioner som är avsedda för stor data analys.  Dessa funktioner är tillgängliga i ett General-Purpose v2-eller BlockBlobStorage-lagrings konto och du kan hämta dem genom att aktivera funktionen **hierarkiskt namn område** för kontot. För skalnings mål, se de här artiklarna. 

- [Skala mål för Blob Storage](../articles/storage/blobs/scalability-targets.md#scale-targets-for-blob-storage).
- [Skala mål för standard lagrings konton](../articles/storage/common/scalability-targets-standard-account.md?toc=%252fazure%252fstorage%252fblobs%252ftoc.json#scale-targets-for-standard-storage-accounts).

**Azure Data Lake Storage gen1** är en dedikerad tjänst. Det är en företagsomfattande storskalig lagrings plats för stora data analys arbets belastningar. Du kan använda Data Lake Storage Gen1 för att samla in data om storlek, typ och inmatnings hastighet på en enda plats för operativa och undersökande analyser. Det finns ingen gräns för mängden data som du kan lagra i ett Data Lake Storage Gen1 konto.

| **Resurs** | **Gräns** | **Kommentarer** |
| --- | --- | --- |
| Maximalt antal Data Lake Storage Gen1-konton, per prenumeration, per region |10 | Kontakta supporten om du vill begära en ökning av den här gränsen. |
| Maximalt antal åtkomst-ACL: er, per fil eller mapp |32 | Detta är en hård gräns. Använd grupper för att hantera åtkomst med färre poster. |
| Maximalt antal standard-ACL: er, per fil eller mapp |32 | Detta är en hård gräns. Använd grupper för att hantera åtkomst med färre poster. |