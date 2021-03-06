---
title: ta med fil
description: ta med fil
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 01/28/2020
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: bcc55156ca1d03614a4ff9767d6cf3f2603c06ca
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027809"
---
Lägg till stöd i önskad utvecklings miljö med hjälp av följande metoder.

| Utvecklingsmiljö  | Programtyp      | För att lägga till stöd |
|--------------------------|-----------------------|----------------|
| Visual Studio            | C#-klass bibliotek      | [Installera NuGet-paketet](../articles/azure-functions/functions-bindings-register.md#vs) |
| Visuell Studio-kod       | Baserat på [kärn verktyg](../articles/azure-functions/functions-run-local.md) | [Registrera tilläggs paketet](../articles/azure-functions/functions-bindings-register.md#extension-bundles)<br><br>Vi rekommenderar att du installerar [tillägget Azure-verktyg](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) . |
| Andra redigerare/IDE     | Baserat på [kärn verktyg](../articles/azure-functions/functions-run-local.md) | [Registrera tilläggs paketet](../articles/azure-functions/functions-bindings-register.md#extension-bundles) |
| Azure-portalen             | Endast online i portalen | Installeras när du lägger till en bindning<br /><br /> Se [Uppdatera dina tillägg](../articles/azure-functions/functions-bindings-register.md) för att uppdatera befintliga bindnings tillägg utan att behöva publicera om din Function-app. |