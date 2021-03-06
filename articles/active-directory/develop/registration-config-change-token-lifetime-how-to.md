---
title: Ändra livstid för token standard för anpassade Azure AD-appar
description: Så här uppdaterar du livs längd principer för ditt program som du utvecklar på Azure AD
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 10/23/2020
ms.author: ryanwi
ms.custom: aaddev, seoapril2019
ms.openlocfilehash: 9483fe972cf1a4dce4fb285ced3cb390d0bda725
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/24/2020
ms.locfileid: "92516791"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>Ändra standardvärdena för token för token för ett anpassat, utvecklat program

Den här artikeln visar hur du använder Azure AD PowerShell för att ange en livs längd princip för token. Azure AD Premium tillåter utvecklare av appar och klient administratörer att konfigurera livs längden för token som utfärdas för icke-konfidentiella klienter. Livs längds principer för token anges per klient eller de resurser som nås.

> [!IMPORTANT]
> Efter den 30 januari 2021 kommer klient organisationer inte längre att kunna konfigurera livstider för uppdatering och sessionstoken och Azure Active Directory slutar att svara på befintlig uppdatering och sessionstoken i principer efter det datumet. Du kan fortfarande konfigurera livstid för åtkomsttoken efter utfasningen. Mer information finns i avsnittet [om livs längder för konfigurerbara token i Azure AD](./active-directory-configurable-token-lifetimes.md).
> Vi har implementerat [hanterings funktioner för autentisering](../conditional-access/howto-conditional-access-session-lifetime.md)   i Azure AD villkorlig åtkomst. Du kan använda den här nya funktionen för att konfigurera livstid för uppdateringstoken genom att ange inloggnings frekvens.  

Om du vill ange en livstids princip för token måste du ladda ned [Azure AD PowerShell-modulen](https://www.powershellgallery.com/packages/AzureADPreview).
Kör kommandot **Connect-AzureAD-Confirm** .

Här är en exempel princip som kräver att användare autentiseras oftare i din webbapp. Den här principen anger livs längden för åtkomst-/ID-token och den maximala åldern för en Multi-Factor session-token till tjänstens huvud namn för din webbapp. Skapa principen och tilldela den till tjänstens huvud namn. Du måste också hämta ObjectId för tjänstens huvud namn.

```powershell
$policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"

$sp = Get-AzureADServicePrincipal -Filter "DisplayName eq '<service principal display name>'"

Add-AzureADServicePrincipalPolicy -Id $sp.ObjectId -RefObjectId $policy.Id
```

## <a name="next-steps"></a>Nästa steg

* Lär dig hur du konfigurerar livs längder för token som utfärdats av Azure AD, inklusive hur du ställer in livs längd för token för alla appar i din organisation, för en app med flera klienter, eller för ett särskilt tjänst huvud namn i din organisation, i [Azure AD](./active-directory-configurable-token-lifetimes.md) . 
* [Referens för Azure AD-token](./id-tokens.md)