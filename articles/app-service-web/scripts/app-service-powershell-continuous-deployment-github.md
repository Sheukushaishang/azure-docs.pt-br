---
title: "Exemplo de script do Azure PowerShell - criar um aplicativo Web com a implantação contínua do GitHub | Microsoft Docs"
description: "Exemplo de script do Azure PowerShell - criar um aplicativo Web com a implantação contínua do GitHub"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 42f901f8-02f7-4869-b22d-d99ef59f874c
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: cephalin
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: c371833219426fbc4a0fd8aaa798a8b44e67672b
ms.contentlocale: pt-br
ms.lasthandoff: 04/27/2017

---

# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>Criar um aplicativo web com a implantação contínua do GitHub

Este script de exemplo cria um aplicativo Web no serviço de aplicativo com seus recursos relacionados e, em seguida, define a implantação contínua de um repositório GitHub. Para implantação do GitHub sem a implantação contínua, veja [Criar um aplicativo Web e implantar o código do GitHub](app-service-powershell-deploy-github.md).

Se necessário, instale o Azure PowerShell usando a instrução encontrada no [guia do Azure PowerShell](/powershell/azure/overview). Além disso, verifique se:

- Uma conexão com o Azure foi criado usando o comando `az login`.
- O código do aplicativo está em um repositório GitHub público ou privado que você possui.
- Você [criou um token de acesso em sua conta do GitHub](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[principal](../../../powershell_scripts/app-service/deploy-github-continuous/deploy-github-continuous.ps1?highlight=1-2 "Criar um aplicativo web com a implantação contínua do GitHub")]

## <a name="clean-up-deployment"></a>Limpar implantação 

Após a execução da amostra de script, o comando a seguir pode ser usado para remover o grupo de recursos, o aplicativo Web e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os seguintes comandos. Cada comando na tabela redireciona para a documentação específica do comando.

| Command | Observações |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Cria um Plano do Serviço de Aplicativo. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Cria um aplicativo web. |
| [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource) | Modifica um recurso em um grupo de recursos. |

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o módulo do Azure PowerShell, confira a [Documentação do Azure PowerShell](/powershell/azure/overview).

Exemplos adicionais do Azure PowerShell para Aplicativos Web do Serviço de Aplicativo do Azure podem ser encontrados nos [exemplos do Azure PowerShell](../app-service-powershell-samples.md).

