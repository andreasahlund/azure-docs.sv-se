---
title: Java Script-ett program scenario med en sida – Microsoft Identity Platform | Azure
description: Lär dig hur du skapar ett program med en enda sida (scenario översikt) med hjälp av Microsoft Identity Platform.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev, identityplatformtop40, devx-track-js
ms.openlocfilehash: 17acb2bc5e96a136f31371c0be912c2c758c0f76
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/10/2020
ms.locfileid: "94443865"
---
# <a name="scenario-single-page-application"></a>Scenario: ett program med en sida

Lär dig allt du behöver för att bygga ett enda webb program (SPA).

## <a name="getting-started"></a>Komma igång

Om du inte redan har gjort det skapar du din första app genom att fylla i Java Script SPA-snabb starten:

[Snabb start: ett program på en sida](./quickstart-v2-javascript.md)

## <a name="overview"></a>Översikt

Många moderna webb program skapas som program på en enda sida med klient sidan. Utvecklare skriver dem med hjälp av Java Script eller ett SPA-ramverk, till exempel vinkel, Vue och reagera. Dessa program körs i en webbläsare och har olika egenskaper för autentisering än traditionella webb program på Server sidan.

Microsoft Identity Platform innehåller **två** alternativ för att aktivera program med en enda sida för att logga in användare och hämta token för att få åtkomst till backend-tjänster eller webb-API: er:

- [OAuth 2,0 Authorization Code Flow (med PKCE)](./v2-oauth2-auth-code-flow.md). Med auktoriseringskod-flödet kan programmet byta auktoriseringskod för **ID-** token för att representera den autentiserade **användare och de åtkomsttoken som** krävs för att anropa skyddade API: er. Dessutom returneras **uppdaterings** -token som ger långsiktig åtkomst till resurser för användare utan att det krävs någon interaktion med dessa användare. Detta är den **rekommenderade** metoden.

![Program med en enda sida – auth](./media/scenarios/spa-app-auth.svg)

- [OAuth 2,0 implicit flöde](./v2-oauth2-implicit-grant-flow.md). Med det implicita tilldelnings flödet kan programmet Hämta **ID-** **och åtkomsttoken** . Till skillnad från flödet för auktoriseringskod returnerar inte det implicita tilldelnings flödet en **uppdateringstoken**.

![Program med en sida – implicit](./media/scenarios/spa-app.svg)

Det här autentiseringsschemat omfattar inte program scenarier som använder plattforms oberoende JavaScript-ramverk som Electron och reagerar-Native. De kräver ytterligare funktioner för interaktion med de ursprungliga plattformarna.

## <a name="specifics"></a>Information

Om du vill aktivera det här scenariot för ditt program behöver du:

* Program registrering med Azure Active Directory (Azure AD). Registrerings stegen skiljer sig åt mellan det implicita tilldelnings flödet och det implicita flödet för utfärdande.
* Program konfiguration med de registrerade program egenskaperna, t. ex. program-ID.
* Använd Microsoft Authentication Library för Java Script (MSAL.js) för att utföra autentiserings flödet för att logga in och hämta token.

## <a name="recommended-reading"></a>Rekommenderad läsning

[!INCLUDE [recommended-topics](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="next-steps"></a>Nästa steg

Gå vidare till nästa artikel i det här scenariot, [Registrera appen](scenario-spa-app-registration.md).
