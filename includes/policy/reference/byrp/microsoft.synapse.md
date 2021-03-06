---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 11/20/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: fb5f6dbb410a15bf88fccc1ed3f9c3ca0f830c70
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/20/2020
ms.locfileid: "96296103"
---
|Namn<br /><sub>(Azure Portal)</sub> |Beskrivning |Påverkan (ar) |Version<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure Synapse-arbetsytor bör använda Kundhanterade nycklar för att kryptera data i vila](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff7d52b2d-e161-4dfa-a82b-55e564167385) |Använd Kundhanterade nycklar för att kontrol lera krypteringen vid resten av de data som lagras i Azure Synapse-arbetsytor. Kundhanterade nycklar levererar Double Encryption genom att lägga till ett andra lager av kryptering ovanpå standard krypteringen med tjänstens hanterade nycklar. |Granska, neka, inaktive rad |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceCMK_Audit.json) |
|[IP-brandväggs regler på Azure Synapse-arbetsytor bör tas bort](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F56fd377d-098c-4f02-8406-81eb055902b8) |Att ta bort alla regler för IP-brandvägg förbättrar säkerheten genom att se till att din Azure Synapse-arbetsyta bara kan nås från en privat slut punkt. Den här konfigurationen granskar skapande av brand Väggs regler som tillåter offentlig nätverks åtkomst på arbets ytan. |Granskning, inaktive rad |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceFirewallRules_Audit.json) |
|[Det virtuella nätverket för hanterad arbets yta på Azure Synapse-arbetsytor måste vara aktiverat](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2d9dbfa3-927b-4cf0-9d0f-08747f971650) |Om du aktiverar ett virtuellt nätverk för hanterade arbets ytor ser du till att arbets ytan är isolerad från andra arbets ytor. Data integrering och Spark-resurser som distribueras i det här virtuella nätverket ger också isolering på användar nivå för Spark-aktiviteter. |Granska, neka, inaktive rad |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceManagedVnet_Audit.json) |
|[Anslutningar för privata slut punkter i Azure Synapse-arbetsytor måste vara aktiverade](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F72d11df1-dd8a-41f7-8925-b05b960ebafc) |Privata slut punkter kan konfigureras för att ansluta privat till en Azure Synapse-arbetsyta. Detta används för att genomdriva en säker kommunikations kanal till Azure dataSynapses-arbetsytan. |Granskning, inaktive rad |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceUsePrivateLinks_Audit.json) |
|[Synapse-hanterade privata slut punkter ska endast ansluta till resurser i godkända Azure Active Directory klienter](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3a003702-13d2-4679-941b-937e58c443f0) |Skydda din Synapse-arbetsyta genom att endast tillåta anslutningar till resurser i godkända Azure Active Directory (Azure AD)-klient organisationer. Godkända Azure AD-klienter kan definieras under princip tilldelning. |Granskning, inaktive rad, neka |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/Workspace_DataExfiltrationPrevention_Deny.json) |
