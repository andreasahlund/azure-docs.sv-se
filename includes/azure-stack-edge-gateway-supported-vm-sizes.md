---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 08/03/2020
ms.author: alkohli
ms.openlocfilehash: 7ce49873b4e59bcf474deaea4420f56a72c9c589
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89085741"
---
Storleken på den virtuella datorn avgör hur mycket beräkningsresurser (t.ex. CPU, GPU och minne) som är tillgängligt för den virtuella datorn. Virtuella datorer måste skapas med en virtuell dator med lämplig storlek för arbetsbelastningen. Även om alla datorer ska köras på samma maskin vara, har dator storlekarna olika begränsningar för disk åtkomst, som kan hjälpa dig att hantera den totala disk åtkomsten över dina virtuella datorer. Om arbetsbelastningen ökar kan man även ändra storlek på den befintliga virtuella datorn.

Följande virtuella datorer i standard Dv2-serien stöds för att skapa Azure Stack Edge-enhet.

### <a name="dv2-series"></a>Dv2-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temp-lagring (GiB)  | Högsta data flöde för OS-diskar (IOPS) | Maximal data flöde för temp-lagring (IOPS) | Maximalt antal data diskar/data flöde (IOPS) | Maximalt antal nätverkskort |
|-------------------|----|----|-----|----|------|------------|---------|
|**Standard_D1_v2** |1   |3.5 |50   |500 |3000  |4 / 4 x 500   |2 |
|**Standard_D2_v2** |2   |7   |100  |500 |6000  |8 / 8 x 500   |2 |
|**Standard_D3_v2** |4   |14  |200  |500 |12000 |16 / 16 x 500 |4 |
|**Standard_D4_v2** |8   |28  |400  |500 |24000 |32 / 32 x 500 |8 |
|**Standard_D5_v2** |16  |56  |800  |500 |48000 |64/64x500 |8 |

### <a name="dsv2-series"></a>DSv2-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temp-lagring (GiB)  | Högsta data flöde för OS-diskar (IOPS) | Maximal data flöde för temp-lagring (IOPS) | Maximalt antal data diskar/data flöde (IOPS) | Maximalt antal nätverkskort |
|--------------------|----|----|----|-----|------|-------------|---------|
|**Standard_DS1_v2** |1   |3.5 |7   |1000 |4000  |4/4x2300   |2 |
|**Standard_DS2_v2** |2   |7   |14  |1000 |8000  |8/8x2300   |2 |
|**Standard_DS3_v2** |4   |14  |28  |1000 |16000 |16/16x2300 |4 |
|**Standard_DS4_v2** |8   |28  |56  |1000 |32000 |32/32x2300 |8 |
|**Standard_DS5_v2** |16  |56  |112 |1000 |64000 |64/64x2300 |8 |

### <a name="dv2-series"></a>Dv2-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temp-lagring (GiB)  | Högsta data flöde för OS-diskar (IOPS) | Maximal data flöde för temp-lagring (IOPS) | Maximalt antal data diskar/data flöde (IOPS) | Maximalt antal nätverkskort |
|--------------------|----|----|-----|----|-------|-------------|---------|
|**Standard_D11_v2** |2   |14  |100  |500 |6000   |8 / 8 x 500    |2 |
|**Standard_D12_v2** |4   |28  |200  |500 |12000  |16 / 16 x 500  |4 |
|**Standard_D13_v2** |8   |56  |400  |500 |24000  |32 / 32 x 500  |8 |
|**Standard_D14_v2** |16  |112 |800  |500 |48000  |64/64x500  |8 |


### <a name="dsv2-series"></a>DSv2-serien
|Storlek     |Virtuell processor     |Minne (GiB) | Temp-lagring (GiB)  | Högsta data flöde för OS-diskar (IOPS) | Maximal data flöde för temp-lagring (IOPS) | Maximalt antal data diskar/data flöde (IOPS) | Maximalt antal nätverkskort |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_DS11_v2** |2   |14  |28   |1000 |8000   |4/4x2300    |2 |
|**Standard_DS12_v2** |4   |28  |56   |1000 |16000  |8/8x2300    |4 |
|**Standard_DS13_v2** |8   |56  |112  |1000 |32000  |16/16x2300  |8 |
|**Standard_DS14_v2** |16  |112 |224  |1000 |64000  |32/32x2300  |8 |

Mer information finns i Dv2- [serien på generell användning VM-storlekar](../articles/virtual-machines/dv2-dsv2-series.md#dv2-series).
