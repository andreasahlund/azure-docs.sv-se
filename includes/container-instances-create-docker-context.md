---
title: inkludera fil
description: inkludera fil
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 08/13/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 3221c4f3e196cf1573bd7c0424fa3b4530c0b2ca
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90709670"
---
## <a name="create-azure-context"></a>Skapa Azure-kontext

Om du vill använda Docker-kommandon för att köra behållare i Azure Container Instances loggar du först in på Azure:

```bash
docker login azure
```

När du uppmanas till det anger eller väljer du dina autentiseringsuppgifter för Azure.


Skapa en ACI-kontext genom att köra `docker context create aci` . Den här kontexten kopplar Docker med en Azure-prenumeration och resurs grupp så att du kan skapa och hantera behållar instanser. Till exempel för att skapa en kontext som heter *myacicontext*:

```
docker context create aci myacicontext
```

När du uppmanas väljer du ditt ID för Azure-prenumerationen och väljer sedan en befintlig resurs grupp eller **skapar en ny resurs grupp**. Om du väljer en ny resurs grupp skapas den med ett namn som skapats av systemet. Azure-containerinstanser måste, precis som alla Azure-resurser, distribueras i en resursgrupp. Resursgrupper gör det enkelt att organisera och hantera relaterade Azure-resurser.


Kör `docker context ls` för att bekräfta att du har lagt till ACI-kontexten till Docker-kontexterna:

```
docker context ls
```