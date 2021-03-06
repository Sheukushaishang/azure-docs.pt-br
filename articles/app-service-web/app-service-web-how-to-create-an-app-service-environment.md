---
title: "Como Criar um Ambiente do Serviço de Aplicativo"
description: "Descrição do fluxo de criação para ambientes do serviço de aplicativo"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 81bd32cf-7ae5-454b-a0d2-23b57b51af47
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2016
ms.author: ccompy
translationtype: Human Translation
ms.sourcegitcommit: af98ae3b425e2e0584da53721249dbcd605a17d1
ms.openlocfilehash: 8d8e1ce7758691a392fdb6d6459e04c6b4f7f5f6


---
# <a name="how-to-create-an-app-service-environment"></a>Como criar um Ambiente do Serviço de Aplicativo
### <a name="overview"></a>Visão geral
Ambientes de Serviço de Aplicativo (ASE) são uma opção de serviço Premium do Serviço de Aplicativo do Azure que fornece um recurso de configuração avançada não disponível em carimbos com vários locatários.  O recurso ASE essencialmente implanta o Serviço de Aplicativo do Azure na rede virtual de um cliente.  Para obter uma maior compreensão dos recursos oferecidos pelos ambientes do serviço de aplicativo, leia a documentação [O que é um ambiente do serviço de aplicativo][WhatisASE].

### <a name="before-you-create-your-ase"></a>Antes de criar seu ASE
É importante estar ciente dos itens que você não pode alterar.  Os aspectos que você não pode alterar quanto ao ASE após sua criação são:

* Local
* Assinatura
* Grupo de recursos
* VNET usada
* Sub-rede usada 
* Tamanho da sub-rede

Ao escolher uma rede virtual e especificar uma sub-rede, verifique se ela é grande o suficiente para acomodar qualquer crescimento futuro.  

### <a name="creating-an-app-service-environment"></a>Criando um Ambiente do Serviço de Aplicativo
Há duas maneiras de acessar a interface do usuário de criação do ASE.  Ela pode ser encontrada pesquisando no Azure Marketplace por ***ambiente de serviço de aplicativo*** ou acessando as opções de menu Novo -> Web + Móvel -> Ambiente do Serviço de Aplicativo.  Para criar um ASE:

1. Forneça o nome do seu ASE.  O nome especificado para o ASE será usado para os aplicativos Web criados no ASE.  Se o nome do ASE for appsvcenvdemo, o nome do subdomínio será .*appsvcenvdemo.p.azurewebsites.net*.  Se você tiver criado, portanto, um aplicativo Web chamado *mytestapp*, ele seria endereçável em *mytestapp.appsvcenvdemo.p.azurewebsites.net*.  Você não pode usar espaços em branco no nome do ASE.  Se você usar letras maiúsculas entre os caracteres do nome, o nome de domínio será a versão total em letras minúsculas desse nome.  Se você usar um ILB, seu nome do ASE não será usado no subdomínio, mas sim declarado explicitamente durante a criação do ASE
   
    ![][1]
2. Selecione sua assinatura.  A assinatura usada para seu ASE também é aquela com a qual serão criados todos os aplicativos no ASE.  Você pode colocar seu ASE em uma VNet que está em outra assinatura
3. Selecione ou especifique um novo grupo de recursos.  O grupo de recursos usado para seu ASE deve ser o mesmo usado para sua rede virtual.  Se você selecionar uma rede virtual já existente, a seleção de grupo de recursos para o ASE será atualizada para refletir a de sua rede virtual.
   
    ![][2]
4. Faça suas seleções de Rede Virtual e Local.  Você pode optar por criar uma nova rede virtual ou selecionar uma rede virtual já existente.  Se selecionar uma nova rede virtual, você poderá especificar um nome e local. A nova VNet terá o intervalo de endereços 192.168.250.0/23 e uma sub-rede denominada **padrão** que é definida como 192.168.250.0/24.  Você pode simplesmente selecionar um VNet pré-existente clássico ou do Gerenciador de Recursos.  A seleção do tipo de VIP determina se seu ASE pode ser acessado diretamente por meio da Internet (Externo) ou se ele usa um Balanceador de Carga Interno (ILB).  Para saber mais sobre eles, leia [Usando um Balanceador de Carga Interno com um Ambiente de Serviço de Aplicativo][ILBASE].  Se você selecionar um tipo de VIP Externo, poderá selecionar com quantos endereços IP externos o sistema é criado para fins de IPSSL.  Se selecionar Interno, você precisará especificar o subdomínio que seu ASE usará.  Os ASEs podem ser implantados em redes virtuais que usam os intervalos de endereço público *ou* espaços de endereço *RFC1918* (ou seja, endereços privados).  Para usar uma rede virtual com um intervalo de endereços públicos, você precisará criar a VNet antecipadamente.  Ao selecionar uma VNet já existente, você precisará criar uma nova sub-rede durante a criação do ASE.  **Você não pode usar uma sub-rede criada previamente no portal.  Você poderá criar um ASE com uma sub-rede já existente, se criar o ASE usando um modelo do Resource Manager.**  Para criar um ASE por meio de um modelo, use as informações fornecidas aqui, [Criar um ambiente de serviço de aplicativo do modelo][ILBAseTemplate] e aqui, [Criar um ambiente de serviço de aplicativo do ILB do modelo][ASEfromTemplate].

### <a name="details"></a>Detalhes
É criado um ASE com dois Front-Ends e dois trabalhos.  Os Front-Ends atuam como os pontos de extremidade HTTP/HTTPS e enviam tráfego para as Funções de Trabalho, que são funções que hospedam seus aplicativos.   Você pode ajustar a quantidade após a criação do ASE e pode até mesmo configurar regras de dimensionamento automático nesses pools de recursos.  Para obter mais detalhes sobre a colocação em escala manual, o gerenciamento e o monitoramento de um ambiente de serviço de aplicativo, acesse: [Como configurar um Ambiente de Serviço de Aplicativo][ASEConfig] 

Pode existir apenas um ASE na sub-rede usada pelo ASE.  A sub-rede não pode ser usada para algo diferente de ASE

### <a name="after-app-service-environment-creation"></a>Após a criação de um Ambiente do Serviço de Aplicativo
Após a criação do ASE é possível ajustar:

* Quantidade de Front-Ends (mínimo: 2)
* Quantidade de processadores (mínimo: 2)
* Quantidade de endereços IP disponíveis para SSL de IP
* Tamanho de recursos de computação usados pelos Front-Ends ou Processadores (o tamanho mínimo de Front-End é P2)

Há mais detalhes sobre o gerenciamento da colocação em escala manual e monitoramento de Ambientes de Serviço de Aplicativo aqui: [Como configurar um Ambiente de Serviço de Aplicativo][ASEConfig] 

Para saber mais sobre o dimensionamento automático, há um guia aqui: [Como configurar o dimensionamento automático para um Ambiente de Serviço de Aplicativo][ASEAutoscale]

Há dependências adicionais que não estão disponíveis para personalização, como o banco de dados e o armazenamento.  Esses são gerenciados pelo Azure e fornecidos com o sistema.  O armazenamento do sistema dá suporte a até 500 GB para todo o Ambiente de Serviço do Aplicativo e o banco de dados é ajustado pelo Azure como necessário, por meio do dimensionamento do sistema.

## <a name="getting-started"></a>Introdução
Todos os artigos e os Como fazer para Ambientes de Serviço de Aplicativo estão disponíveis no [LEIAME para Ambientes de Serviço de Aplicativo](../app-service/app-service-app-service-environments-readme.md).

Para se familiarizar com os ambientes de serviço de aplicativo, consulte [Introdução aos ambientes de Serviço de Aplicativo][WhatisASE]

Para obter mais informações sobre a plataforma de Serviço de Aplicativo do Azure, consulte [Serviço de Aplicativo do Azure][AzureAppService].

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/



<!--HONumber=Feb17_HO3-->


