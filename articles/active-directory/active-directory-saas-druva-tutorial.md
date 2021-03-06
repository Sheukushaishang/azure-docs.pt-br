---
title: "Tutorial: integração do Azure Active Directory ao Druva | Microsoft Docs"
description: "Saiba como usar o Druva com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 325d92e493f6e011367d2c85b52c92838327101e
ms.openlocfilehash: 0c24b01924d5fcec0d4600e044e679cfc295815f
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Tutorial: integração do Active Directory do Azure ao Druva
O objetivo deste tutorial é mostrar a integração do Azure ao Druva.  

O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Uma assinatura válida do Azure
* Uma assinatura habilitada para SSO (logon único) do Druva

Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao Druva poderão fazer logon único no aplicativo em seu site de empresa do Druva (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

* Habilitando a integração de aplicativos para Druva
* Configurando o logon único
* Configurando o provisionamento de usuários
* Atribuindo usuários

![Cenário](./media/active-directory-saas-druva-tutorial/IC795084.png "Cenário")

## <a name="enable-the-application-integration-for-druva"></a>Habilitar a integração de aplicativos para Druva
O objetivo desta seção é descrever como habilitar a integração de aplicativos para o Druva.

**Para habilitar a integração de aplicativos para o Druva, execute as seguintes etapas:**

1. No Portal clássico do Azure, no painel de navegação à esquerda, clique em **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-druva-tutorial/IC700993.png "Active Directory")
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
   ![Aplicativos](./media/active-directory-saas-druva-tutorial/IC700994.png "Aplicativos")
4. Clique em **Adicionar** na parte inferior da página.
   
   ![Adicionar aplicativo](./media/active-directory-saas-druva-tutorial/IC749321.png "Adicionar aplicativo")
5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
   ![Adicionar um aplicativo da galeria](./media/active-directory-saas-druva-tutorial/IC749322.png "Adicionar um aplicativo da galeria")
6. Na **caixa de pesquisa**, digite **Druva**.
   
   ![Galeria de Aplicativos](./media/active-directory-saas-druva-tutorial/IC795085.png "Galeria de Aplicativos")
7. No painel de resultados, selecione **Druva** e clique em **Concluir** para adicionar o aplicativo.
   
   ![Druva](./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
   
## <a name="configure-single-sign-on"></a>Configurar o logon único

O objetivo desta seção é descrever como permitir que os usuários autentiquem no Druva com a própria conta no Azure AD usando federação baseada no protocolo SAML.  

Como parte deste procedimento, é necessário criar um arquivo de certificado codificado em base-64. Se você não estiver familiarizado com esse procedimento, veja [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).

Seu aplicativo Druva espera as declarações do SAML em um formato específico, o que exige que você adicione mapeamentos de atributo personalizados de acordo com a sua configuração de **atributos do token SAML** . 

A captura de tela a seguir mostra um exemplo disso.

![Atributos de Token SAML](./media/active-directory-saas-druva-tutorial/IC795087.png "Atributos de Token SAML")

**Para configurar o logon único, execute as seguintes etapas:**

1. No portal clássico do Azure, na página de integração do aplicativo **Druva**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.
   
   ![Configurar Logon Único](./media/active-directory-saas-druva-tutorial/IC795027.png "Configurar Logon Único")
2. Na página **Como você deseja que os usuários façam logon no Druva**, selecione **Logon Único do Microsoft Azure AD** e clique em **Avançar**.
   
   ![Configurar Logon Único](./media/active-directory-saas-druva-tutorial/IC795088.png "Configurar Logon Único")
3. Na página **Configurar a URL do Aplicativo**, na caixa de texto **URL de Entrada no Druva**, digite a URL usada pelos usuários para fazer logon no seu aplicativo Druva (por exemplo: "*https://cloud.druva.com/home/*”) e clique em **Avançar**.
   
   ![Configurar URL do Aplicativo](./media/active-directory-saas-druva-tutorial/IC795089.png "Configurar URL do Aplicativo")
4. Na página **Configurar logon único no Druva**, para baixar seu certificado, clique em **Baixar certificado** e salve o arquivo de certificado localmente no computador.
   
   ![Configurar Logon Único](./media/active-directory-saas-druva-tutorial/IC795090.png "Configurar Logon Único")
5. Em outra janela do navegador da Web, faça logon em seu site de empresa do Druva como administrador.
6. Vá para **Gerenciar \> Configurações**.
   
   ![Configurações](./media/active-directory-saas-druva-tutorial/IC795091.png "Configurações")
7. Na caixa de diálogo Configurações de Logon Único, execute as seguintes etapas:
   
   ![Configurações de Logon Único](./media/active-directory-saas-druva-tutorial/IC795092.png "Configurações de Logon Único")   
   1. No portal clássico do Azure, na página do diálogo **Configurar logon único no Druva**, copie o valor da **URL de Logon Remoto** e cole-o na caixa de texto **URL de Logon do Provedor de Identidade**.
   2. No portal clássico do Azure, na página do diálogo **Configurar logon único no Druva**, copie o valor da **URL de Logout Remoto** e cole-o na caixa de texto **URL de Logout do Provedor de Identidade**.
   3. Crie um arquivo **codificado em base&64;** usando o certificado baixado.  
     >[!TIP]
     >Para obter mais detalhes, veja [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o)
     > 
   4. Abra seu certificado codificado em Base&64; no bloco de notas, copie o conteúdo dele na área de transferência e cole-o na caixa de texto **Certificado do Provedor de ID**
   5. Para abrir a página **Configurações**, clique em **Salvar**.
8. Na página **Configurações**, clique em **Gerar Token de SSO**.
   
   ![Configurações](./media/active-directory-saas-druva-tutorial/IC795093.png "Configurações")
9. No diálogo **Token de Autenticação de Logon Único** , realize as seguintes etapas:
   
   ![Token SSO](./media/active-directory-saas-druva-tutorial/IC795094.png "Token SSO")
   1. Clique em **Copiar**.
   2. Clique em **fechar**
10. No portal clássico do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar logon único**.
    
   ![Configurar Logon Único](./media/active-directory-saas-druva-tutorial/IC795095.png "Configurar Logon Único")
11. Na parte superior do menu, clique em **Atributos** to open the **SAML Token Atributos** .
    
    ![Atributos](./media/active-directory-saas-druva-tutorial/IC795096.png "Atributos")
12. Para adicionar os mapeamentos de atributo necessários, execute as seguintes etapas:
    
   | Nome do atributo | Valor do atributo |
   | --- | --- |
   | insync\_auth\_token |<*valor da área de transferência*> |
    
   1. Para cada linha de dados na tabela acima, clique em **adicionar atributo do usuário**.
   2. Na caixa de texto **Nome do Atributo** , digite o nome do atributo mostrado para a linha.
   3. Na caixa de texto **Valor do Atributo** , digite o valor do atributo mostrado para essa linha.
   4. Clique em **Concluído**.
13. Clique em **Aplicar alterações**.
    
## <a name="configure-user-provisioning"></a>Configurar provisionamento do usuário

Para permitir que os usuários do Azure AD façam logon no Druva, eles devem ser provisionados no Druva. 

* No caso do Druva, o provisionamento é uma tarefa manual.

**Para configurar o provisionamento de usuários, execute as seguintes etapas:**

1. Faça logon em seu site de empresa do **Druva** como administrador.
2. Vá para **Gerenciar \> Usuários**.
   
   ![Gerenciar Usuários](./media/active-directory-saas-druva-tutorial/IC795097.png "Gerenciar Usuários")
3. Clique em **Criar Novo**.
   
   ![Gerenciar Usuários](./media/active-directory-saas-druva-tutorial/IC795098.png "Gerenciar Usuários")
4. Na caixa de diálogo Criar Novo Usuário, execute as seguintes etapas:
   
   ![Criar Novo Usuário](./media/active-directory-saas-druva-tutorial/IC795099.png "Criar Novo Usuário")
   
   1. Digite o endereço de email e o nome da conta de usuário válida do Active Directory do Azure, que você deseja provisionar nas caixas de texto relacionadas.
   2. Clique em **Criar Usuário**.

>[!NOTE]
>É possível usar qualquer outra ferramenta de criação da conta de usuário do Druva ou as APIs fornecidas pelo Druva para provisionar as contas de usuário do AAD.
> 
> 

## <a name="assign-users"></a>Atribuir usuários
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que usem seu aplicativo.

**Para atribuir usuários ao Druva, execute as etapas a seguir:**

1. No Portal clássico do Azure, crie uma conta de teste.
2. Na página de integração do aplicativo **Druva**, clique em **Atribuir usuários**.
   
   ![Atribuir Usuários](./media/active-directory-saas-druva-tutorial/IC795100.png "Atribuir Usuários")
3. Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.
   
   ![Sim](./media/active-directory-saas-druva-tutorial/IC767830.png "Sim")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).


