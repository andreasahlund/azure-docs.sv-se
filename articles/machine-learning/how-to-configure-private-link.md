---
title: Konfigurera en privat slut punkt
titleSuffix: Azure Machine Learning
description: Använd Azures privata länk för att få säker åtkomst till din Azure Machine Learning-arbetsyta från ett virtuellt nätverk.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to, devx-track-azurecli
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/30/2020
ms.openlocfilehash: 2953f85a5c21cdd670d6e133d09ffacf06f178ef
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/18/2020
ms.locfileid: "94842710"
---
# <a name="configure-azure-private-link-for-an-azure-machine-learning-workspace"></a>Konfigurera en privat Azure-länk för en Azure Machine Learning-arbetsyta

I det här dokumentet får du lära dig hur du använder en privat Azure-länk med din Azure Machine Learning-arbetsyta. Information om hur du skapar ett virtuellt nätverk för Azure Machine Learning finns i [Översikt över virtuell nätverks isolering och sekretess](how-to-network-security-overview.md)

Med Azures privata länk kan du ansluta till din arbets yta med en privat slut punkt. Den privata slut punkten är en uppsättning privata IP-adresser i det virtuella nätverket. Du kan sedan begränsa åtkomsten till din arbets yta så att den bara sker över de privata IP-adresserna. Privat länk hjälper till att minska risken för data exfiltrering. Mer information om privata slut punkter finns i artikeln [Azure Private Link](../private-link/private-link-overview.md) .

> [!IMPORTANT]
> Azures privata länk påverkar inte Azures kontroll plan (hanterings åtgärder) som att ta bort arbets ytan eller hantera beräknings resurser. Till exempel skapa, uppdatera eller ta bort ett beräknings mål. De här åtgärderna utförs via det offentliga Internet som normalt. Data Plans åtgärder som att använda Azure Machine Learning Studio, API: er (inklusive publicerade pipeliner) eller SDK använder den privata slut punkten.
>
> Du kan stöta på problem vid försök att komma åt den privata slut punkten för din arbets yta om du använder Mozilla Firefox. Det här problemet kan vara relaterat till DNS via HTTPS i Mozilla. Vi rekommenderar att du använder Microsoft Edge av Google Chrome som en lösning.

## <a name="prerequisites"></a>Förutsättningar

Om du planerar att använda en privat länk aktive rad arbets yta med en kundhanterad nyckel måste du begära denna funktion med hjälp av ett support ärende. Mer information finns i [Hantera och öka kvoter](how-to-manage-quotas.md#private-endpoint-and-private-dns-quota-increases).

## <a name="limitations"></a>Begränsningar

Det går inte att använda en Azure Machine Learning arbets yta med privat länk i Azure Government regioner eller Azure Kina 21Vianet-regioner.

## <a name="create-a-workspace-that-uses-a-private-endpoint"></a>Skapa en arbets yta som använder en privat slut punkt

Använd någon av följande metoder för att skapa en arbets yta med en privat slut punkt. Var och en av dessa metoder __kräver ett befintligt virtuellt nätverk__:

> [!TIP]
> Om du vill skapa en arbets yta, en privat slut punkt och ett virtuellt nätverk samtidigt, se [Använd en Azure Resource Manager-mall för att skapa en arbets yta för Azure Machine Learning](how-to-create-workspace-template.md).

# <a name="python"></a>[Python](#tab/python)

Azure Machine Learning python SDK tillhandahåller klassen [PrivateEndpointConfig](/python/api/azureml-core/azureml.core.privateendpointconfig?view=azure-ml-py) , som kan användas med [arbets ytan. Create ()](/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---tags-none--friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--adb-workspace-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--private-endpoint-config-none--private-endpoint-auto-approval-true--exist-ok-false--show-output-true-) för att skapa en arbets yta med en privat slut punkt. Den här klassen kräver ett befintligt virtuellt nätverk.

```python
from azureml.core import Workspace
from azureml.core import PrivateEndPointConfig

pe = PrivateEndPointConfig(name='myprivateendpoint', vnet_name='myvnet', vnet_subnet_name='default')
ws = Workspace.create(name='myworkspace',
    subscription_id='<my-subscription-id>',
    resource_group='myresourcegroup',
    location='eastus2',
    private_endpoint_config=pe,
    private_endpoint_auto_approval=True,
    show_output=True)
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[Azure CLI-tillägget för Machine Learning](reference-azure-machine-learning-cli.md) tillhandahåller kommandot [AZ ml arbets yta skapa](/cli/azure/ext/azure-cli-ml/ml/workspace?view=azure-cli-latest#ext_azure_cli_ml_az_ml_workspace_create) . Följande parametrar för det här kommandot kan användas för att skapa en arbets yta med ett privat nätverk, men det krävs ett befintligt virtuellt nätverk:

* `--pe-name`: Namnet på den privata slut punkt som skapas.
* `--pe-auto-approval`: Om de privata slut punkts anslutningarna till arbets ytan automatiskt ska godkännas.
* `--pe-resource-group`: Resurs gruppen som den privata slut punkten ska skapas i. Måste vara samma grupp som innehåller det virtuella nätverket.
* `--pe-vnet-name`: Det befintliga virtuella nätverket som den privata slut punkten ska skapas i.
* `--pe-subnet-name`: Namnet på det undernät som den privata slut punkten ska skapas i. Standardvärdet är `default`.

# <a name="portal"></a>[Portal](#tab/azure-portal)

På fliken __nätverk__ i Azure Machine Learning Studio kan du konfigurera en privat slut punkt. Det kräver dock ett befintligt virtuellt nätverk. Mer information finns i [skapa arbets ytor i portalen](how-to-manage-workspace.md).

---

## <a name="add-a-private-endpoint-to-a-workspace"></a>Lägga till en privat slut punkt i en arbets yta

Använd någon av följande metoder för att lägga till en privat slut punkt till en befintlig arbets yta:

> [!IMPORTANT]
>
> Du måste ha ett befintligt virtuellt nätverk för att skapa den privata slut punkten i. Du måste också [inaktivera nätverks principer för privata slut punkter](../private-link/disable-private-endpoint-network-policy.md) innan du lägger till den privata slut punkten.

> [!WARNING]
>
> Om du har några befintliga beräknings mål som är kopplade till den här arbets ytan och de inte ligger bakom samma virtuella nätverks Tha som den privata slut punkten skapas i, kommer de inte att fungera.

# <a name="python"></a>[Python](#tab/python)

```python
from azureml.core import Workspace
from azureml.core import PrivateEndPointConfig

pe = PrivateEndPointConfig(name='myprivateendpoint', vnet_name='myvnet', vnet_subnet_name='default')
ws = Workspace.from_config()
ws.add_private_endpoint(private_endpoint_config=pe, private_endpoint_auto_approval=True, show_output=True)
```

Mer information om de klasser och metoder som används i det här exemplet finns i [PrivateEndpointConfig](/python/api/azureml-core/azureml.core.privateendpointconfig?view=azure-ml-py) och [Workspace.add_private_endpoint](/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#add-private-endpoint-private-endpoint-config--private-endpoint-auto-approval-true--location-none--show-output-true--tags-none-).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[Azure CLI-tillägget för Machine Learning](reference-azure-machine-learning-cli.md) ger [AZ ml-arbetsytans tillägg för privat slut punkt](/cli/azure/ext/azure-cli-ml/ml/workspace/private-endpoint?view=azure-cli-latest#ext_azure_cli_ml_az_ml_workspace_private_endpoint_add) .

```azurecli
az ml workspace private-endpoint add -w myworkspace  --pe-name myprivateendpoint --pe-auto-approval true --pe-vnet-name myvnet
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

I arbets ytan Azure Machine Learning på portalen väljer du __privata slut punkter anslutningar__ och väljer sedan __+ privat slut punkt__. Använd fälten för att skapa en ny privat slut punkt.

* När du väljer __region__ väljer du samma region som det virtuella nätverket. 
* Använd __Microsoft. MachineLearningServices/arbets ytor__ när du väljer __resurs typ__. 
* Ange __resursen__ till arbets ytans namn.

Välj slutligen __skapa__ för att skapa den privata slut punkten.

---

## <a name="remove-a-private-endpoint"></a>Ta bort en privat slut punkt

Använd någon av följande metoder för att ta bort en privat slut punkt från en arbets yta:

# <a name="python"></a>[Python](#tab/python)

Använd [Workspace.delete_private_endpoint_connection](/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#delete-private-endpoint-connection-private-endpoint-connection-name-) för att ta bort en privat slut punkt.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
# get the connection name
_, _, connection_name = ws.get_details()['privateEndpointConnections'][0]['id'].rpartition('/')
ws.delete_private_endpoint_connection(private_endpoint_connection_name=connection_name)
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[Azure CLI-tillägget för Machine Learning](reference-azure-machine-learning-cli.md) ger [AZ ml-arbetsytans borttagnings kommando för ta bort slut punkt](/cli/azure/ext/azure-cli-ml/ml/workspace/private-endpoint?view=azure-cli-latest#ext_azure_cli_ml_az_ml_workspace_private_endpoint_delete) .

# <a name="portal"></a>[Portal](#tab/azure-portal)

Välj __anslutningar för privata slut punkter__ från Azure Machine Learning arbets yta i portalen och välj sedan den slut punkt som du vill ta bort. Välj slutligen __ta bort__.

---

## <a name="using-a-workspace-over-a-private-endpoint"></a>Använda en arbets yta över en privat slut punkt

Eftersom kommunikation till arbets ytan endast tillåts från det virtuella nätverket måste alla utvecklings miljöer som använder arbets ytan vara medlemmar i det virtuella nätverket. Till exempel en virtuell dator i det virtuella nätverket.

> [!IMPORTANT]
> För att undvika tillfälligt avbrott i anslutningen rekommenderar Microsoft att tömma DNS-cachen på datorer som ansluter till arbets ytan när du har aktiverat privat länk. 

Information om Azure Virtual Machines finns i Virtual Machines- [dokumentationen](../virtual-machines/index.yml).


## <a name="next-steps"></a>Nästa steg

* Mer information om hur du skyddar din Azure Machine Learning-arbetsyta finns i artikeln om [isolering av virtuella nätverk och sekretess översikt](how-to-network-security-overview.md) .

* Om du planerar att använda en anpassad DNS-lösning i ditt virtuella nätverk, se [hur du använder en arbets yta med en anpassad DNS-Server](how-to-custom-dns.md).
