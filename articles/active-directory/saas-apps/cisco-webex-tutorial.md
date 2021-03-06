---
title: 'Tutorial: Integração do Azure Active Directory ao Cisco Webex Meetings | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Cisco Webex Meetings.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/01/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 308f745489fba2e2b539a2f2615b65228565dcf9
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65900119"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex-meetings"></a>Tutorial: Tutorial: integração do Azure Active Directory ao Cisco Webex Meetings

Neste tutorial, você aprenderá como integrar o Cisco Webex Meetings ao Azure AD (Azure Active Directory).
A integração do Cisco Webex Meetings ao Azure AD oferece os seguintes benefícios:

* Você pode controlar no Azure AD quem tem acesso ao Cisco Webex Meetings.
* Você pode permitir que os usuários sejam conectados automaticamente ao Cisco Webex Meetings (Logon Único) com suas contas do Azure AD.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Cisco Webex Meetings, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/)
* Assinatura do Cisco Webex Meetings habilitada para logon único

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Cisco Webex Meetings dá suporte ao SSO iniciado por **SP**

* O Cisco Webex Meetings dá suporte ao provisionamento de usuário **Just-In-Time**

## <a name="adding-cisco-webex-meetings-from-the-gallery"></a>Adicionando o Cisco Webex Meetings da galeria

Para configurar a integração do Cisco Webex Meetings ao Azure AD, você precisa adicionar o Cisco Webex Meetings da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Cisco Webex Meetings por meio da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Cisco Webex Meetings**, selecione **Cisco Webex Meetings** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

     ![Cisco Webex Meetings na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Cisco Webex Meetings, com base em um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Cisco Webex Meetings.

Para configurar e testar o logon único do Azure AD com o Cisco Webex Meetings, você precisará concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o Logon Único do Cisco Webex Meetings](#configure-cisco-webex-meetings-single-sign-on)** – para definir as configurações de Logon Único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Cisco Webex Meetings](#create-cisco-webex-meetings-test-user)** – para ter um equivalente de Brenda Fernandes no Cisco Webex Meetings que esteja vinculado à representação de usuário do Azure AD.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Cisco Webex Meetings, execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Cisco Webex Meetings**, selecione **Logon único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. No portal do Azure, na página **Configurar Logon Único com o SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração Básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na seção **Configuração Básica do SAML**, carregue o arquivo **Metadados do Provedor de Serviços** baixado e configure o aplicativo executando as seguintes etapas:

    >[!Note]
    >Você obterá o arquivo de metadados do provedor de serviço, que é explicado posteriormente na seção **Configurar o logon único do Cisco Webex Meetings** do tutorial. 

     a. Clique em **Carregar arquivo de metadados**.

    ![Carregar arquivo de metadados](common/upload-metadata.png)

    b. Clique no **logotipo da pasta** para selecionar o arquivo de metadados e depois em **Carregar**.

    ![Escolha o arquivo de metadados](common/browse-upload-metadata.png)

    c. Após a conclusão bem-sucedida do upload do arquivo de metadados do Provedor de Serviços, os valores de **Identificador** e **URL de Resposta** são preenchidos automaticamente na seção **Configuração Básica do SAML**:

    ![Informações de logon único em Domínio e URLs do Cisco Webex Meetings](common/sp-identifier-reply.png)

    Na caixa de texto **URL de logon**, cole o valor da **URL de resposta** que é preenchida automaticamente por upload do arquivo de metadados de SP.

5. O aplicativo Cisco Webex Meetings espera as declarações SAML em um formato específico, o que exige a adição de mapeamentos de atributo personalizado à configuração de atributos do token SAML. A captura de tela a seguir mostra a lista de atributos padrão. Clique no ícone **Editar** para adicionar os atributos.

    ![image](common/edit-attribute.png)

6. Exclua os atributos padrão da seção **Declarações de usuário** e o aplicativo Cisco Webex Meetings espera que mais alguns atributos sejam passados de volta na resposta SAML. Na seção **Declarações de Usuário** da caixa de diálogo **Atributos de Usuário**, execute as seguintes etapas para adicionar o atributo de token SAML, conforme mostrado na tabela abaixo:
    
    | NOME | Atributo de Origem|
    | ---------------|  --------- |
    |   nome    | user.givenname |
    |   sobrenome    | user.surname |
    |   email       | user.mail |
    |   uid    | user.mail |

     a. Clique em **Adicionar nova reivindicação** para abrir a caixa de diálogo **Gerenciar declarações de usuários**.

    ![image](./media/cisco-webex-tutorial/tutorial-cisco-webex-addnewclaim.png)

    ![image](common/new-attribute-details.png)

    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.

    c. Deixe o **Namespace** em branco.

    d. Escolha Origem como **Atributo**.

    e. Na lista **Atributo de origem**, digite o valor do atributo mostrado para essa linha.

    f. Clique em **Ok**

    g. Clique em **Save** (Salvar).

7. Na página **Configurar Logon Único com SAML**, na seção **Certificado de Autenticação SAML**, clique em **Baixar** para baixar o **XML de Metadados de Federação** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/metadataxml.png)

8. Na seção **Configurar o Cisco Webex Meetings**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-cisco-webex-meetings-single-sign-on"></a>Configurar o Logon Único do Cisco Webex Meetings

1. Entre no [Gerenciamento de Colaboração de Nuvem da Cisco](https://www.webex.com/go/connectadmin) com suas credenciais de administrador.

2. Vá para **Configurações de Segurança** e navegue até **Configuração de SSO da Web Federado**.
 
    ![Configurar o logon único](./media/cisco-webex-tutorial/tutorial-cisco-webex-10.png)

3. Em **Configuração de SSO da Web Federado**, execute as seguintes etapas:

    ![Configurar o logon único](./media/cisco-webex-tutorial/tutorial-cisco-webex-11.png)

     a. Na caixa de texto Protocolo de Federação, digite o nome do seu protocolo.

    b. Clique no link **Importar metadados SAML** para carregar o arquivo de metadados que você baixou do portal do Azure.

    c. Clique no botão **Exportar** para baixar o arquivo de metadados do provedor de serviço e carregue-o na seção **Configuração básica de SAML** no portal do Azure.

    d. Na caixa de texto **AuthContextClassRef**, digite `urn:oasis:names:tc:SAML:2.0:ac:classes:unspecified` e, se você deseja habilitar o MFA usando o Azure AD, digite os dois valores como `urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport;urn:oasis:names:tc:SAML:2.0:ac:classes:X509`

    e. Selecione **Criação Automática de Conta**.

    >[!NOTE]
    >Para habilitar o provisionamento de usuário **Just-In-Time**, você precisa marcar a **Criação Automática de Conta**. Além disso, os atributos do token SAML precisam ser passados na resposta SAML.

    f. Clique em **Save** (Salvar). 

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory**, selecione **Usuários** e, em seguida, **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

    a. No campo **Nome**, insira **BrendaFernandes**.
  
    b. No campo **Nome de usuário**, digite **brendafernandes\@dominiodaempresa.extensao**  
    Por exemplo, BrittaSimon@contoso.com

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo a ela o acesso ao Cisco Webex Meetings.

1. No portal do Azure, selecione **Aplicativos Empresariais**, **Todos os aplicativos** e, em seguida, **Cisco Webex Meetings**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, escolha **Cisco Webex Meetings**.

    ![O link do Cisco Webex Meetings na lista de Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-cisco-webex-meetings-test-user"></a>Criar um usuário de teste do Cisco Webex Meetings

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no Cisco Webex Meetings. O Cisco Webex Meetings dá suporte ao provisionamento **Just-In-Time**, que é habilitado por padrão. Não há itens de ação para você nesta seção. Se um usuário ainda não existir no Cisco Webex Meetings, um novo será criado quando você tentar acessar o Cisco Webex Meetings.

### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Cisco Webex Meetings no Painel de Acesso, você deverá ser conectado automaticamente ao Cisco Webex Meetings para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

