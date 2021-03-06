---
title: "Criar uma função no Azure disparada por um webhook do GitHub | Microsoft Docs"
description: "Usar o Azure Functions para criar uma função sem servidor que é invocada por um webHook do GitHub."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 8f987d079b8658d591994ce678f4a09239270181
ms.openlocfilehash: d79ce0e047e71d9f6af7ca55f55bea405c280b1d
ms.contentlocale: pt-br
ms.lasthandoff: 05/18/2017


---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>Criar uma função disparada pelo webhook do GitHub

Saiba como criar uma função que é disparada por uma solicitação de webhook HTTP com um conteúdo específico do GitHub.

![Função disparada pelo webhook do GitHub no Portal do Azure](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Pré-requisitos

Antes de executar este exemplo, você deve ter o seguinte:

- Uma conta do GitHub com pelo menos um projeto.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Criar um Aplicativo de funções do Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplicativo de funções criado com êxito.](./media/functions-create-first-azure-function/function-app-create-success.png)

Em seguida, crie uma nova função no novo aplicativo de funções.

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a>Criar uma função disparada pelo webhook do GitHub

1. Expanda seu aplicativo de funções, clique no botão **+** ao lado de **Funções**, clique no modelo **GitHubWebHook** para a linguagem de programação desejada. **Nomeie a função** e, em seguida, clique em **Criar**.

1. Na sua nova função, clique em **</> Obter URL de função** e depois copie e salve os valores. Faça o mesmo para **</> Get GitHub secret**. Você usa esses valores ao criar o webhook no GitHub.

    ![Examinar o código de função](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

Em seguida, você cria o webhook no repositório GitHub.

## <a name="configure-the-webhook"></a>Configurar o webhook

1. No GitHub, navegue até um repositório de sua propriedade. Você também pode usar qualquer repositório que você tenha bifurcado. Se você precisar bifurcar um repositório, use <https://github.com/Azure-Samples/functions-quickstart>.

1. Clique em **Configurações**, em seguida, clique em **Webhooks**, e **Adicionar webhook**.

    ![Adicionar um webhook do GitHub](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. Use as configurações especificadas na tabela e, em seguida, clique em **Adicionar webhook**.

    ![Definir o segredo e a URL do webhook](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| Configuração | Valor sugerido | Descrição |
|---|---|---|
| **URL do conteúdo** | Valor copiado | Usar o valor retornado por **</> Obter URL de função**. |
| **Segredo**   | Valor copiado | Use o valor retornado por **</> Obter segredo do GitHub**. |
| **Tipo de conteúdo** | aplicativo/json | A função espera um conteúdo JSON. |
| Gatilhos de evento | Deixe-me selecionar eventos individuais | Queremos disparar apenas em eventos de comentário do problema.  |
| | Comentário do problema |  |

Neste momento, o webhook é configurado para disparar sua função quando um novo comentário do problema é adicionado.

## <a name="test-the-function"></a>Testar a função

1. No seu repositório GitHub, abra a guia **Problemas** em uma nova janela do navegador.

1. Na nova janela, clique em **Novo Problema**, digite um título e clique em **Enviar novo problema**.

1. No problema, digite um comentário e clique em **Comentar**.

    ![Adicione um comentário de problema do GitHub.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. Volte para o portal e exiba os logs. Você deve ver uma entrada de rastreamento com o novo texto de comentário.

     ![Exiba o texto do comentário nos logs.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Próximas etapas

Você criou uma função que é executada quando uma solicitação é recebida de um webhook do GitHub. 
[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)] Para obter mais informações sobre gatilhos do webhook, consulte [Associações HTTP e de webhook do Azure Functions](functions-bindings-http-webhook.md).
