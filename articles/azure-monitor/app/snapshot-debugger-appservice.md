---
title: Aktivera Snapshot Debugger för .NET-appar i Azure App Service | Microsoft Docs
description: Aktivera Snapshot Debugger för .NET-appar i Azure App Service
ms.topic: conceptual
author: brahmnes
ms.author: bfung
ms.date: 03/26/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 6928da704236c4bb5492f99a4a5327bf297a323d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84676858"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-app-service"></a>Aktivera Snapshot Debugger för .NET-appar i Azure App Service

Snapshot Debugger fungerar för närvarande för ASP.NET och ASP.NET Core appar som körs på Azure App Service i Windows Service-planer.

## <a name="enable-snapshot-debugger"></a><a id="installation"></a> Aktivera Snapshot Debugger
Följ anvisningarna nedan om du vill aktivera Snapshot Debugger för en app. Om du kör en annan typ av Azure-tjänst, finns här instruktioner för att aktivera Snapshot Debugger på andra plattformar som stöds:
* [Azure Cloud Services](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Service Fabric-tjänster](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Azure Virtual Machines och skalnings uppsättningar för virtuella datorer](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)
* [Lokala virtuella eller fysiska datorer](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json)

Om du använder en för hands version av .NET Core följer du anvisningarna för [att aktivera Snapshot debugger för andra miljöer](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) för att först inkludera paketet [Microsoft. ApplicationInsights. SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet med programmet och sedan slutföra resten av anvisningarna nedan. 

Application Insights Snapshot Debugger är förinstallerat som en del av App Services runtime, men du måste aktivera det för att skapa ögonblicks bilder för din App Service-app. När du har distribuerat en app, även om du har inkluderat Application Insights SDK i käll koden, följer du stegen nedan för att aktivera fel sökning av ögonblicks bild.

1. Navigera till Azures kontroll panel för din App Service.
2. Gå till sidan **inställningar > Application Insights** .

   ![Aktivera App Insights på App Services Portal](./media/snapshot-debugger/applicationinsights-appservices.png)

3. Följ anvisningarna på sidan för att skapa en ny resurs eller Välj en befintlig App Insights-resurs för att övervaka din app. Se också till att båda växlar för Snapshot Debugger finns **på**.

   ![Lägg till App Insights-webbplatsens tillägg][Enablement UI]

4. Snapshot Debugger har nu Aktiver ATS med en inställning för App Services app.

    ![App-inställning för Snapshot Debugger][snapshot-debugger-app-setting]

## <a name="disable-snapshot-debugger"></a>Inaktivera Snapshot Debugger

Följ samma steg som för **aktivering av Snapshot debugger**, men växla båda växlar för Snapshot debugger till **av**.
Vi rekommenderar att du har Snapshot Debugger aktiverat i alla dina appar för att under lätta diagnostiken av program undantag.

## <a name="azure-resource-manager-template"></a>Azure Resource Manager-mall

För en Azure App Service kan du ange inställningar för appar i en Azure Resource Manager mall för att aktivera Snapshot Debugger och profiler. Du lägger till en konfigurations resurs som innehåller appinställningar som en underordnad resurs till webbplatsen:

```json
{
  "apiVersion": "2015-08-01",
  "name": "[parameters('webSiteName')]",
  "type": "Microsoft.Web/sites",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  "tags": { 
    "[concat('hidden-related:', resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName')))]": "empty",
    "displayName": "Website"
  },
  "properties": {
    "name": "[parameters('webSiteName')]",
    "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
  },
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "name": "appsettings",
      "type": "config",
      "dependsOn": [
        "[parameters('webSiteName')]",
        "[concat('AppInsights', parameters('webSiteName'))]"
      ],
      "properties": {
        "APPINSIGHTS_INSTRUMENTATIONKEY": "[reference(resourceId('Microsoft.Insights/components', concat('AppInsights', parameters('webSiteName'))), '2014-04-01').InstrumentationKey]",
        "APPINSIGHTS_PROFILERFEATURE_VERSION": "1.0.0",
        "APPINSIGHTS_SNAPSHOTFEATURE_VERSION": "1.0.0",
        "DiagnosticServices_EXTENSION_VERSION": "~3",
        "ApplicationInsightsAgent_EXTENSION_VERSION": "~2"
      }
    }
  ]
},
```

## <a name="next-steps"></a>Nästa steg

- Generera trafik till ditt program som kan utlösa ett undantag. Vänta sedan 10 till 15 minuter för ögonblicks bilder som ska skickas till Application Insights-instansen.
- Se [ögonblicks bilder](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) i Azure Portal.
- Hjälp med fel sökning Snapshot Debugger problem finns i [Snapshot debugger fel sökning](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).

[Enablement UI]: ./media/snapshot-debugger/enablement-ui.png
[snapshot-debugger-app-setting]:./media/snapshot-debugger/snapshot-debugger-app-setting.png

