---
title: "Operações de Azure AD Connect Health."
description: "Este artigo descreve as outras operações que podem ser executadas após a implantação do Azure AD Connect Health."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 86cc3840-60fb-43f9-8b2a-8598a9df5c94
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/31/2017
ms.author: vakarand
translationtype: Human Translation
ms.sourcegitcommit: e4d2c77fca602661ae3e061c1c0fc84ebb786d32
ms.openlocfilehash: 1c43589455e8f5c744538634d1eaad7a7f05cf5d


---
# <a name="azure-ad-connect-health-operations"></a>Operações de Azure AD Connect Health
O tópico a seguir descreve as várias operações que podem ser executadas usando o Azure AD Connect Health.

## <a name="enable-email-notifications"></a>Habilitar notificações por email
Você pode configurar o serviço do Azure AD Connect Health para enviar notificações por email quando os alertas forem gerados, indicando que sua infraestrutura não está íntegra. Isso ocorrerá quando um alerta for gerado, bem como quando ele é marcado como resolvido. Siga as instruções abaixo para configurar notificações por email.

![Descoberta de notificações por email do Azure AD Connect Health](./media/active-directory-aadconnect-health/email_noti_discover.png)

> [!NOTE]
> As notificações por email são desabilitadas por padrão.
>
>

### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Para habilitar notificações por Email do Azure AD Connect Health
1. Abra a folha Alertas do serviço para o qual você deseja receber notificação por email.
2. Clique no botão "Configurações de notificação" na barra de ação.
3. Ative a opção de Notificação por email como ON.
4. Marque a caixa de seleção para configurar todos os administradores globais para receber notificações por email.
5. Se você deseja receber notificações por email em qualquer outro endereço de email, pode especificá-los na caixa Destinatário de Email Adicional. Para remover um endereço de email da lista, clique com o botão direito na entrada e selecione Excluir.
6. Para finalizar as alterações clique em "Salvar". Todas as alterações entrarão em vigor somente depois que você selecionar "Salvar".

## <a name="delete-a-server-or-service-instance"></a>Excluir uma instância de serviço ou servidor

Em alguns casos, você poderá remover um servidor que está sendo monitorado. Siga as instruções abaixo para remover um servidor do serviço de Azure AD Connect Health.

Ao excluir um servidor, esteja ciente das seguintes opções:

* Esta ação irá PARAR de coletar todos os demais dados desse servidor. Esse servidor será removido do serviço de monitoramento. Após essa ação, você não poderá exibir os novos alertas, monitoramento ou o uso de dados de análise para esse servidor.
* Essa ação NÃO irá desinstalar ou remover o agente de integridade do servidor. Se você não tiver desinstalado o agente de integridade antes de executar essa etapa, será possível ver os eventos de erro no servidor relacionados ao agente de integridade.
* Essa ação NÃO excluirá os dados já coletados deste servidor. Esses dados serão excluídos de acordo com a política de retenção de dados do Microsoft Azure.
* Depois de executar essa ação, se você quiser iniciar o monitoramento do mesmo servidor novamente, será necessário desinstalar e reinstalar o agente de integridade neste servidor.

### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>Para excluir um servidor do serviço do Azure AD Connect Health
Azure AD Connect Health para AD FS e Azure AD Connect (Sincronização):

1. Abra a folha do servidor da folha da lista do servidor, selecionando o nome do servidor a ser removido.
2. Na folha do servidor, clique no botão "Excluir" na barra de ação.
3. Confirme a ação para excluir o servidor digitando o nome do servidor na caixa de confirmação.
4. Clique no botão "Excluir".

Azure AD Connect Health para AD DS:

1. Abra o painel Controladores de Domínio.
2. Selecione o controlador de domínio a ser removido.
3. Clique no botão "Excluir Selecionado" na barra de ação.
4. Confirme a ação para excluir o servidor.
5. Clique no botão "Excluir".

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Exclua uma instância de serviço do serviço do Azure AD Connect Health Service
Em alguns casos, será possível remover uma instância do serviço. Siga as instruções abaixo para remover uma instância de serviço do Azure AD Connect Health.

Ao excluir uma instância de serviço, esteja ciente do seguinte:

* Esta ação irá remover a instância do serviço atual do serviço de monitoramento.
* Essa ação NÃO irá desinstalar ou remover o agente de integridade de nenhum um dos servidores que foram monitorados como parte desta instância de serviço. Se você não tiver desinstalado o agente de integridade antes de executar essa etapa, poderá ver os eventos de erro no(s) servidor(es) relacionado(s) ao agente de integridade.
* Todos os dados dessa instância do serviço serão excluídos de acordo com a política de retenção de dados do Microsoft Azure.
* Depois de executar essa ação, se você quiser iniciar o serviço de monitoramento, desinstale e reinstale o agente de integridade em todos os servidores que serão monitorados. Depois de executar essa ação, se você quiser iniciar o monitoramento do mesmo servidor novamente, será necessário desinstalar e reinstalar o agente de integridade neste servidor.

#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>Para excluir uma instância de serviço do Azure AD Connect Health Service
1. Abra uma folha de serviço na folha da lista do serviço, selecionando o identificador de serviço (nome do farm) que você deseja remover.
2. Na folha do servidor, clique no botão "Excluir" na barra de ação.
3. Confirme o nome do serviço digitando-o na caixa de confirmação. (por exemplo: sts.contoso.com)
4. Clique no botão "Excluir".
   <br><br>

[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Gerenciar o acesso com o controle de acesso baseado em função
### <a name="overview"></a>Visão geral
[Controle de acesso baseado em função](../role-based-access-control-configure.md) para Azure AD Connect Health fornece acesso ao serviço Azure AD Connect Health para usuários e/ou grupos fora dos administradores globais. Isso é feito atribuindo funções aos grupos e/ou aos usuários desejados e fornece um mecanismo para limitar os administradores globais no diretório.

#### <a name="roles"></a>Funções
O Azure AD Connect Health suporta as seguintes funções internas.

| Função | Permissões |
| --- | --- |
| Proprietário |Os proprietários podem ***gerenciar o acesso*** (por exemplo, atribuir função a um usuário/grupo), ***exibir todas as informações*** (por exemplo, exibir alertas) a partir do portal e ***alterar as configurações*** (por exemplo, notificações por email) no Azure AD Connect Health. <br>Por padrão, os administradores globais do AD do Azure são atribuídos a essa função e isso não pode ser alterado. |
| Colaborador |Os colaboradores podem ***exibir todas as informações*** (por exemplo, exibir alertas) a partir do portal e ***alterar as configurações*** (por exemplo, notificações por email) no Azure AD Connect Health. |
| Leitor |Os leitores podem ***exibir todas as informações*** (por exemplo, exibir alertas) no portal do Azure AD Connect Health. |

Todas as outras funções (como 'Administradores de acesso do usuário' ou 'Usuários de laboratórios de testes e desenvolvimento'), mesmo que estejam disponíveis na experiência do portal, não têm impacto de acesso no Azure AD Connect Health.

#### <a name="access-scope"></a>Escopo de acesso
O Azure AD Connect oferece suporte ao gerenciamento de acesso em dois níveis:

* ***Todas as instâncias de serviço***: este é o caminho recomendado para a maioria dos clientes e controla o acesso para todas as instâncias do serviço (por exemplo, um farm do ADFS) em todos os tipos de função monitorados pelo Azure AD Connect Health.
* ***Instância de serviço***: em alguns casos, talvez seja necessário separar o acesso com base em tipos de função ou por uma instância de serviço. Nesse caso, você pode gerenciar o acesso no nível da instância de serviço.  

A permissão é concedida se um usuário final tem acesso no nível do diretório ou da instância do serviço.

### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>Como permitir o acesso de usuários ou grupos ao Azure AD Connect Health
#### <a name="steps-1-select-the-appropriate-access-scope"></a>Etapa 1: Selecione o escopo de acesso apropriado
Para permitir a um usuário o acesso no nível *Todas as instâncias de serviço* no Azure AD Connect Health, abra a folha principal no Azure AD Connect Health.<br>

#### <a name="step-2-add-users-groups-and-assign-roles"></a>Etapa 2: Adicione usuários, grupos e atribua funções
1. Clique na parte "Usuários" na seção Configurar.<br>
   ![Folha Principal do RBAC do Azure AD Connect Health](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Selecione "Adicionar"
3. Selecione a "Função", por exemplo, "Proprietário"<br>
   ![Adicionar Usuários do RBAC do Azure AD Connect Health](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Digite o nome ou identificador do usuário ou grupo de destino. Você pode selecionar um ou mais usuários ou grupos ao mesmo tempo. Clique em "selecionar".
   ![Selecionar Usuário do RBAC do Azure AD Connect Health](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Selecione "Ok".<br>
6. Uma vez concluída a atribuição da função, os usuários e/ou grupos aparecerão na lista.<br>
   ![Lista de Usuários do RBAC do Azure AD Connect Health](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Essas etapas permitirão acesso aos usuários e grupos listados de acordo com suas funções.

> [!NOTE]
> * Os Administradores Globais sempre têm acesso completo a todas as operações, mas as contas de administrador global não estarão presentes na lista acima.
> * NÃO há suporte para o recurso “Convidar Usuários” no Azure AD Connect Health.
>
>

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Etapa 3: Compartilhe o local da folha com usuários ou grupos
1. Depois de atribuir permissões, um usuário poderá acessar o Azure AD Connect Health em [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth).
2. Uma vez na folha, o usuário poderá fixar a folha, ou partes diferentes, no painel clicando em "Fixar no painel"<br>
   ![Fixar folha do RBAC do Azure AD Connect Health](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)

> [!NOTE]
> Um usuário com a função "Leitor" atribuída não poderá executar a operação "criar" para obter a extensão do Azure AD Connect Health no Azure Marketplace. Esse usuário ainda pode obter a folha indo para o link acima. Para uso subsequente, o usuário pode fixar a folha no painel.
>
>

### <a name="remove-users-andor-groups"></a>Remover usuários e/ou grupos
Você pode remover um usuário ou grupo adicionado à parte de controle de acesso baseado em função do Azure AD Connect Health clicando com botão direito e selecionando remover.<br>
![Remover Usuário do RBAC do Azure AD Connect Health](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>Links relacionados
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalação do Agente do Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Usando o Azure AD Connect Health com o AD FS](active-directory-aadconnect-health-adfs.md)
* [Usando o Azure AD Connect Health para sincronização](active-directory-aadconnect-health-sync.md)
* [Usar o Azure AD Connect Health com o AD DS](active-directory-aadconnect-health-adds.md)
* [Perguntas frequentes do Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Histórico de versão do Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)


<!--HONumber=Dec16_HO3-->


