---
title: Examinar o acesso às funções do AD do Azure no PIM - Azure Active Directory | Microsoft Docs
description: Saiba como examinar acesso das funções do Azure AD no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3850af026ea13e1920aea65e18358ebb04ef0d25
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65140977"
---
# <a name="review-access-to-azure-ad-roles-in-pim"></a>Examinar o acesso às funções do AD do Azure no PIM

Azure Active Directory (AD) Privileged Identity Management (PIM) simplifica como as empresas gerenciam o acesso privilegiado a recursos no Azure AD e outros Microsoft online services como Office 365 ou Microsoft Intune.  

Se você for atribuído a uma função administrativa, o administrador de função com privilégios de sua organização poderá solicitar que você confirme regularmente que ainda precisa da função para seu trabalho. Você pode receber um email que inclui um link ou pode acessar diretamente o [portal do Azure](https://portal.azure.com). Siga as etapas neste artigo para executar a autorrevisão das suas funções atribuídas.

Se você for um administrador com privilégios de função ou um administrador global interessado em revisões de acesso, obtenha mais detalhes em [Como iniciar uma revisão de acesso](pim-how-to-start-security-review.md).

## <a name="add-the-privileged-identity-management-application"></a>Adicionar o aplicativo Privileged Identity Management
Você pode usar o aplicativo Azure AD PIM (Privileged Identity Management) no [portal do Azure](https://portal.azure.com/) para executar a revisão.  Se você não tiver o aplicativo Azure AD Privileged Identity Management em seu portal, siga estas etapas para começar.

1. Entre no [Portal do Azure](https://portal.azure.com/).
2. Selecione seu nome de usuário no canto superior direito do portal do Azure e selecione o diretório em que você vai operar.
3. Selecione **Todos os serviços** e use a caixa de texto Filtrar para pesquisar o **Azure AD Privileged Identity Management**.
4. Marque **Fixar no painel** e então clique em **Criar**. O aplicativo Privileged Identity Management será aberto.

## <a name="approve-or-deny-access"></a>Aprovar ou negar acesso
Ao aprovar ou negar o acesso, você está apenas dizendo ao revisor se ainda usa essa função ou não. Escolha **Aprovar** se você quiser manter a função ou **Negar** se não precisar mais do acesso. Seu status não mudará imediatamente até que o revisor aplique os resultados.
Siga estas etapas para localizar e concluir a análise de acesso:

1. No aplicativo PIM, selecione **Examinar o acesso com privilégios**. Se você tiver quaisquer análises de acesso pendentes, elas aparecerão na folha de análises do Acesso do Azure AD.
2. Selecione a análise que deseja concluir.
3. A menos que tenha criado a análise, você aparece como o único usuário na análise. Selecione a marca de seleção ao lado de seu nome.
4. Escolha **Aprovar** ou **Negar**. Talvez seja necessário incluir um motivo para a sua decisão na caixa de texto **Fornecer um motivo** .  
5. Feche a folha **Funções de análise do AD do Azure** .

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Próximas etapas

- [Realizar uma revisão de acesso das minhas funções de recurso do Azure no PIM](pim-resource-roles-perform-access-review.md)
