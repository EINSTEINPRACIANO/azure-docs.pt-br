---
title: 'Tutorial: Integração do Azure Active Directory ao Cherwell | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Cherwell.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: ad891f99-179e-4487-834d-35f3bc01c1ec
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 211e11b5bfd6a8bb7b1d61fec120566d60b9c5c0
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65191203"
---
# <a name="tutorial-azure-active-directory-integration-with-cherwell"></a>Tutorial: Integração do Azure Active Directory ao Cherwell

Neste tutorial, você aprende a integrar o Cherwell ao Azure AD (Azure Active Directory).
A integração do Cherwell ao Azure AD oferece os seguintes benefícios:

* Você pode controlar no Azure AD quem terá acesso ao Cherwell.
* Você pode permitir que os usuários sejam conectados automaticamente ao Cherwell (Logon Único) com suas contas do Azure AD.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Cherwell, você precisa dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, poderá obter uma [conta gratuita](https://azure.microsoft.com/free/)
* Assinatura habilitada para logon único do Cherwell

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Cherwell dá suporte ao SSO iniciado por **SP**

## <a name="adding-cherwell-from-the-gallery"></a>Adicionando o Cherwell por meio da galeria

Para configurar a integração do Cherwell ao Azure AD, você precisa adicionar o Cherwell à lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o Cherwell por meio da galeria, realize as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Cherwell**, selecione **Cherwell** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Cherwell na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Cherwell, com base em um usuário de teste chamado **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Cherwell.

Para configurar e testar o logon único do Azure AD com o Cherwell, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o Logon Único do Cherwell](#configure-cherwell-single-sign-on)** – para definir as configurações de Logon Único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Cherwell](#create-cherwell-test-user)** – para ter um equivalente de Brenda Fernandes no Cherwell que esteja vinculado à representação de usuário do Azure AD.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Cherwell, execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Cherwell**, selecione **Logon único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na seção **Configuração Básica do SAML**, execute a seguinte etapa:

    ![Informações de logon único de Domínio e URLs do Cherwell](common/sp-signonurl.png)

    Na caixa de texto **URL de logon**, digite um URL usando o seguinte padrão: `https://<companyname>.cherwellondemand.com/cherwellclient`

    > [!NOTE]
    > O valor não é real. Atualize o valor com a URL de Logon real. Contate a [equipe de suporte ao Cliente do Cherwell](https://cherwellsupport.com/CherwellPortal) para obter o valor. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

5. Na página **Configurar logon único com SAML**, na seção **Certificado de Autenticação SAML**, clique em **Fazer o download** para fazer o download do **Certificado (Base64)** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/certificatebase64.png)

6. Na seção **Configurar o Cherwell**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-cherwell-single-sign-on"></a>Configurar o Logon Único do Cherwell

Para configurar o logon único no lado do **Cherwell**, é necessário enviar o **Certificado (Base64)** baixado e as URLs apropriadas copiadas do portal do Azure para a [equipe de suporte do Cherwell](https://cherwellsupport.com/CherwellPortal). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

> [!NOTE]
> A equipe de suporte do Cherwell precisa fazer a configuração real do SSO. Você receberá uma notificação quando o SSO tiver sido habilitado para sua assinatura.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory**, selecione **Usuários** e, em seguida, **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

     a. No campo **Nome**, insira **BrendaFernandes**.
  
    b. No campo **Nome de usuário**, digite `brittasimon\@yourcompanydomain.extension`. Por exemplo, BrittaSimon@contoso.com.

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao Cherwell.

1. No portal do Azure, selecione **Aplicativos Empresariais**, **Todos os aplicativos** e, em seguida, **Cherwell**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Cherwell**.

    ![O link do Cherwell na lista Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-cherwell-test-user"></a>Criar um usuário de teste do Cherwell

Para permitir os usuários do Azure AD entrem no Cherwell, eles deverão ser provisionados no Cherwell. No caso do Cherwell, as contas de usuário precisam ser criadas pela [equipe de suporte do Cherwell](https://cherwellsupport.com/CherwellPortal).

> [!NOTE]
> Você pode usar qualquer outra ferramenta de criação da conta de usuário do Cherwell ou as APIs fornecidas pelo Cherwell para provisionar as contas de usuário do Active Directory do Azure.

### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Cherwell no Painel de Acesso, você deverá ser conectado automaticamente ao Cherwell, para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
