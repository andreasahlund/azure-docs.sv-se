---
title: inkludera fil
description: inkludera fil
services: storage
author: codemillmatt
ms.service: storage
ms.topic: include
ms.date: 11/23/2019
ms.author: masoucou
ms.custom: include file
ms.openlocfilehash: 02586d38903c60ba8982753ca0bd3e15192d5deb
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/05/2020
ms.locfileid: "83006239"
---
### <a name="copy-your-credentials-from-the-azure-portal"></a>Kopiera dina autentiseringsuppgifter från Azure-portalen

När exempel programmet gör en begäran för att Azure Storage, måste det vara auktoriserat. Om du vill auktorisera en begäran lägger du till autentiseringsuppgifterna för ditt lagrings konto i programmet som en anslutnings sträng. Visa autentiseringsuppgifterna för lagringskontot genom att följa dessa steg:

1. Logga in på [Azure Portal](https://portal.azure.com).
2. Leta rätt på ditt lagringskonto.
3. Välj **Åtkomstnycklar** i avsnittet **Inställningar** i lagringskontoöversikten. Här kan du visa åtkomstnycklarna för kontot och den fullständiga anslutningssträngen för varje nyckel.
4. Sök efter värdet för **Anslutningssträng** under **key1** och kopiera anslutningssträngen genom att välja **Kopiera**. Du lägger till strängvärdet för anslutningen till en miljövariabel i nästa steg.

    ![Skärmbild som visar hur man kopierar en anslutningssträng från Azure-portalen](./media/storage-copy-connection-string-portal/portal-connection-string.png)

### <a name="configure-your-storage-connection-string"></a>Konfigurera anslutningssträngen för lagring

När du har kopierat anslutnings strängen ställer du in den på en variabel på klass nivå i din *mainpage.XAML.cs* -fil. Öppna *MainPaage.XAML.cs* och hitta `storageConnectionString` variabeln. Ersätt `<yourconnectionstring>` med den faktiska anslutnings strängen.

Här är koden:

```csharp
string storageConnectionString = "<yourconnectionstring>";
```