---
title: "Gerenciar o cluster do Azure Kubernetes com a interface do usuário da Web | Microsoft Docs"
description: "Usando a interface do usuário da Web Kubernetes no Serviço de Contêiner do Azure"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: bburns
translationtype: Human Translation
ms.sourcegitcommit: 2a381431acb6436ddd8e13c69b05423a33cd4fa6
ms.openlocfilehash: 5987b1034fc9c52b13606c469683adff06729984
ms.lasthandoff: 02/22/2017


---

# <a name="using-the-kubernetes-web-ui-with-azure-container-service"></a>Usando a interface do usuário da Web Kubernetes com o Serviço de Contêiner do Azure

## <a name="prerequisites"></a>Pré-requisitos
Este passo a passo presume que você tenha [criado um cluster Kubernetes usando o Serviço de contêiner do Azure](container-service-kubernetes-walkthrough.md).


Isso também pressupõe que você tenha a CLI do Azure 2.0 e as ferramentas `kubectl` instaladas.

Você pode testar se tem a ferramenta `az` instalada executando:

```console
$ az --version
```

Se você não tem a ferramenta `az` instalada, há instruções [aqui](https://github.com/azure/azure-cli#installation).

Você pode testar se tem a ferramenta `kubectl` instalada executando:

```console
$ kubectl version
```

Se não tem `kubectl` instalado, você pode executar:

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a>Visão geral

### <a name="connect-to-the-web-ui"></a>Conecte-se à interface do usuário da Web
Você pode iniciar a interface do usuário Web Kubernetes executando:

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

Isso deve abrir um navegador da Web configurado para se comunicar com um proxy seguro, conectando o computador local à interface do usuário Web Kubernetes.

### <a name="create-and-expose-a-service"></a>Criar e expor um serviço
1. Na interface do usuário da Web Kubernetes, clique no botão **Criar** na janela superior direita.

    ![Criar interface do usuário Kubernetes](media/k8s/create.png)

    Isso abrirá uma caixa de diálogo em que você poderá começar a criar o aplicativo.

2. Atribua o nome `hello-nginx`. Use o [`nginx` contêiner de Docker](https://hub.docker.com/_/nginx/) e implante três réplicas do serviço Web.

    ![Caixa de diálogo Criar Pod Kubernetes](media/k8s/nginx.png)

3. Em **Serviço**, selecione **Externo** e insira a porta 80.

    Essa configuração realiza o balanceamento de carga de tráfego para as três réplicas.

    ![Caixa de diálogo Criar serviço Kubernetes](media/k8s/service.png)

4. Clique em **Implantar** para implantar esses contêineres e serviços.

    ![Implantar Kubernetes](media/k8s/deploy.png)

### <a name="view-your-containers"></a>Exibir seus contêineres
Depois que você pressionar **Implantar**, a interface do usuário mostrará uma exibição do serviço que ela implanta:

![Status do Kubernetes](media/k8s/status.png)

Você pode ver o status de cada objeto Kubernetes no círculo no lado esquerdo da interface do usuário, em **Pods**. Se for um círculo parcialmente cheio, o objeto ainda está sendo implantado. Quando um objeto é totalmente implantado, ele exibe uma marca de seleção verde:

![Kubernetes implantado](media/k8s/deployed.png)

Quando tudo estiver em execução, clique em um dos Pods para ver detalhes sobre o serviço Web em execução.

![Pods Kubernetes](media/k8s/pods.png)

Na exibição **Pods**, você pode ver informações sobre os contêineres no pod, bem como os recursos de CPU e memória usados por esses contêineres:

![Recursos do Kubernetes](media/k8s/resources.png)

Se não encontrar os recursos, você precisará aguardar alguns minutos para propagar os dados de monitoramento.

Para ver os logs para o contêiner, clique em **Exibir logs**.

![Logs do Kubernetes](media/k8s/logs.png)

### <a name="viewing-your-service"></a>Exibindo o serviço
Além de executar seus contêineres, a interface do usuário Kubernetes criou um `Service` externo que provisiona um balanceador de carga para trazer o tráfego para os contêineres no cluster.

No painel de navegação esquerdo, clique em **Serviços** para exibir todos os serviços (deve haver apenas um).

![Serviços Kubernetes](media/k8s/service-deployed.png)

Nessa exibição, você deve ver um ponto de extremidade externo (endereço IP) que foi alocado para o serviço.
Se clicar no endereço IP, você deverá ver o contêiner nginx em execução por trás do balanceador de carga.

![nginx view](media/k8s/nginx-page.png)

### <a name="resizing-your-service"></a>Redimensionar o serviço
Além de exibir os objetos na interface do usuário, você também pode editar e atualizar os objetos da API Kubernetes.

Primeiro, clique em **Implantações** no painel de navegação à esquerda para ver a implantação do seu serviço.

Quando você estiver nessa exibição, clique no conjunto de réplicas e clique no botão **Editar** na barra de navegação superior:

![Editar Kubernetes](media/k8s/edit.png)

Edite o campo `spec.replicas` para que seja `2` e clique em **Atualizar**.

Isso fará com que o número de réplicas diminua para duas por meio da exclusão de um de seus pods.

 


