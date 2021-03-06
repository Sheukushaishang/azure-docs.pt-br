---
title: "Implantar recursos do Azure em vários grupos de recursos | Microsoft Docs"
description: "Mostra como escolher mais de um grupo de recursos do Azure durante a implantação."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/11/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: 1436b39fdb9a66a00903442496cc5203b47c1bcb
ms.contentlocale: pt-br
ms.lasthandoff: 05/12/2017


---

# <a name="deploy-azure-resources-to-more-than-one-resource-group"></a>Implantar recursos do Azure em mais de um grupo de recursos

Normalmente, você deve implantar todos os recursos em seu modelo em um único grupo de recursos. No entanto, há situações em que você deseja implantar um conjunto de recursos juntos, mas colocá-los em grupos de recursos diferentes. Por exemplo, você talvez queira implantar a máquina virtual de backup do Azure Site Recovery para um local e um grupo de recursos separados. O Resource Manager permite que você use modelos aninhados para grupos de recursos diferentes do grupo de recursos usado no modelo pai.

O grupo de recursos é o contêiner de ciclo de vida do aplicativo e de sua coleção de recursos. Crie o grupo de recursos fora do modelo e especifique o grupo de recursos de destino durante a implantação. Para obter uma introdução sobre grupos de recursos, confira [Visão geral do Azure Resource Manager](resource-group-overview.md).

## <a name="example-template"></a>Modelo de exemplo

Para buscar um recurso diferente, você deve usar um modelo aninhado ou vinculado durante a implantação. O tipo de recurso `Microsoft.Resources/deployments` fornece um parâmetro `resourceGroup` que permite que você especifique um grupo de recursos diferente para a implantação aninhada. Todos os grupos de recursos devem existir antes da execução da implantação. O exemplo a seguir implanta duas contas de armazenamento: uma no grupo de recursos especificado durante a implantação e outra em um grupo de recursos denominado `crossResourceGroupDeployment`:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName1": {
            "type": "string"
        },
        "StorageAccountName2": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "crossResourceGroupDeployment",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[parameters('StorageAccountName2')]",
                            "apiVersion": "2015-06-15",
                            "location": "West US",
                            "properties": {
                                "accountType": "Standard_LRS"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('StorageAccountName1')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ]
}
```

Se você definir `resourceGroup` como o nome de um grupo de recursos que não existe, a implantação falhará. Se você não fornecer um valor para `resourceGroup`, o Resource Manager usará o grupo de recursos pai.  

## <a name="deploy-the-template"></a>Implantar o modelo

Para implantar o modelo de exemplo, você poderá usar o Azure PowerShell ou a CLI do Azure. Você deve usar uma versão do Azure PowerShell ou da CLI do Azure de maio de 2017 em diante. Os exemplos pressupõem que você salvou o modelo localmente como um arquivo denominado **crossrgdeployment.json**.

Para o PowerShell:

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

Para a CLI do Azure:

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

Após a implantação ser concluída, você verá dois grupos de recursos. Cada grupo de recursos contém uma conta de armazenamento.

## <a name="next-steps"></a>Próximas etapas

* Para entender como definir parâmetros em seu modelo, confira [Noções básicas de estrutura e sintaxe dos modelos do Azure Resource Manager](resource-group-authoring-templates.md).
* Para dicas sobre como resolver erros de implantação, consulte [Solução de erros comuns de implantação do Azure com o Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Para obter mais informações sobre a implantação de um modelo que exija um token SAS, veja [Implantar modelo particular com o token SAS](resource-manager-powershell-sas-token.md).

