---
title: "Saiba como usar o conector do Barramento de Serviço do Azure nos seus aplicativos lógicos | Microsoft Docs"
description: "Crie aplicativos lógicos com o serviço de Aplicativo do Azure. Conecte-se ao Barramento de Serviço do Azure para enviar e receber mensagens. Você pode executar ações como enviar para a fila, enviar para o tópico, receber da fila e receber da assinatura."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/02/2016
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: e5d3301c90adf993b1cba35969dfe4dbeaeab499
ms.contentlocale: pt-br
ms.lasthandoff: 05/10/2017


---
# <a name="get-started-with-the-azure-service-bus-connector"></a>Introdução ao conector do Barramento de Serviço do Azure
Conecte-se ao Barramento de Serviço do Azure para enviar e receber mensagens. Você pode executar ações como enviar para a fila, enviar para o tópico, receber da fila e receber da assinatura.

Para usar [qualquer conector](apis-list.md), primeiro é preciso criar um aplicativo lógico. Você pode começar [criando um aplicativo lógico agora mesmo](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-service-bus"></a>Conectar-se ao Barramento de Serviço
Para que o aplicativo lógico possa acessar qualquer serviço, crie primeiro uma conexão com o serviço. Uma [conexão](connectors-overview.md) fornece uma conectividade entre um aplicativo lógico e outro serviço.  

> [!INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a>Usar um gatilho do Barramento de Serviço
Um gatilho é um evento que pode ser usado para iniciar o fluxo de trabalho definido em um aplicativo lógico. [Saiba mais sobre gatilhos](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a>Usar uma ação de Barramento de Serviço
Uma ação é uma operação executada pelo fluxo de trabalho definido em um aplicativo lógico. [Saiba mais sobre ações](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

[!INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="view-the-swagger"></a>Exibir o Swagger
Consulte os [detalhes do Swagger](/connectors/servicebus/). 

## <a name="next-steps"></a>Próximas etapas
[Criar um aplicativo lógico](../logic-apps/logic-apps-create-a-logic-app.md).


