---
title: Editar uma base de dados de conhecimento – QnA Maker
titleSuffix: Azure Cognitive Services
description: O QnA Maker permite que você gerencie o conteúdo de sua base de dados de conhecimento, fornecendo uma experiência de edição fácil de usar.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 05/10/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: c9add80b7494ae2a8e671967a96dc5d3c7307f51
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65541627"
---
# <a name="edit-a-knowledge-base-in-qna-maker"></a>Editar uma base de dados de conhecimento no QnA Maker

O QnA Maker permite que você gerencie o conteúdo de sua base de dados de conhecimento, fornecendo uma experiência de edição fácil de usar.

<a name="add-datasource"></a>

## <a name="edit-your-knowledge-base-content"></a>Editar o conteúdo da base de conhecimento

1.  Selecione **Minhas bases de dados de conhecimento** na barra de navegação superior. 

    Você pode ver todos os serviços que você criou ou compartilhou classificados em ordem decrescente da data da **última modificação**.

    ![Minhas Bases de Dados de Conhecimento](../media/qnamaker-how-to-edit-kb/my-kbs.png)

1. Selecione uma base de dados de conhecimento específica para fazer edições nela.
 
1. Escolha a opção **Configurações**. Aqui você pode editar o campo obrigatório Nome do Serviço.
  
    |Meta|Ação|
    |--|--|
    |Adicionar URL|Você pode adicionar novos URLs para adicionar novos conteúdos de perguntas frequentes à base de conhecimento clicando em **Gerenciar base de conhecimento -> link "+ Adicionar URL"**.|
    |Excluir URL|Você pode excluir URLs existentes selecionando o ícone de exclusão, a lixeira.|
    |Atualizar o conteúdo da URL|Se você quiser que sua base de conhecimento rastreie o conteúdo mais recente de URLs existentes, marque a caixa de seleção **Atualizar**. Isso atualizará a base de conhecimento com o conteúdo mais recente da URL.|
    |Adicionar arquivo|Você pode adicionar um documento de arquivo suportado para fazer parte de uma base de conhecimento, selecionando **Gerenciar base de conhecimento** e, em seguida, selecionando **+ Adicionar Arquivo**|
    |Importar|Você também pode importar qualquer base de conhecimento existente, selecionando o botão **Base de dados de Conhecimento Ímport**. |
    |Atualizar|A atualização da base de conhecimento depende do **nível de preço de gerenciamento** usado durante a criação do serviço QnA Maker associado à sua base de conhecimento. Você também pode atualizar o tipo de gerenciamento no portal do Azure, se necessário.

1. Quando terminar de fazer alterações na base de conhecimento, selecione **Salvar e treinar** no canto superior direito da página para manter as alterações.    

    ![Salvar e Treinar](../media/qnamaker-how-to-edit-kb/save-and-train.png)

    >[!CAUTION]
    >Se você sair da página antes de selecionar **salvar e treinar**, todas as alterações serão perdidas.

## <a name="add-a-qna-pair"></a>Adicionar um par de QnA

Sobre o **as configurações** página, selecione **QnA adicionar par** para adicionar uma nova linha à tabela de base de Conhecimento.

![Adicionar par QnA](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

## <a name="delete-a-qna-pair"></a>Excluir um par de QnA

Para excluir um QnA, clique no ícone **excluir** à direita da linha QnA. Essa é uma operação permanente. Não pode ser desfeito. Considere exportar seu KB da página **publicar** antes de excluir pares. 

![Excluir par de QnA](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="add-alternate-questions"></a>Adicionar perguntas alternativas

Adicione perguntas alternativas para um par de QnA existente para melhorar a probabilidade de uma correspondência para uma consulta de usuário.

![Adicionar Perguntas Alternativas](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="add-metadata"></a>Adicionar metadados

Adicionar pares de metadados, selecionando primeiro **as opções de exibição**, em seguida, selecionando **Mostrar metadados**. Isso exibe a coluna de metadados. Em seguida, selecione a **+** para adicionar um par de metadados. Esse par consiste em uma chave e um valor.

![Adicionar Metadados](../media/qnamaker-how-to-edit-kb/add-metadata.png)

> [!TIP]
> Salve e treine periodicamente a base de dados de conhecimento depois de fazer edições para evitar a perda de alterações.

## <a name="manage-large-knowledge-bases"></a>Gerenciar grandes bases de conhecimento

* **Grupos de fonte de dados**: Os QnAs são agrupados pela fonte de dados a partir da qual foram extraídos. Você pode expandir ou recolher a fonte de dados.

    ![Usar a barra de fonte de dados do QnA Maker para recolher e expandir perguntas e respostas da fonte de dados](../media/qnamaker-how-to-edit-kb/data-source-grouping.png)

* **Pesquisar na base de dados de conhecimento**: É possível pesquisar a base de dados de conhecimento, digitando a caixa de texto na parte superior da tabela da Base de Dados de Conhecimento. Clique em entrar para pesquisar a pergunta, a resposta ou o conteúdo de metadados. Clique no ícone X para remover o filtro de pesquisa.

    ![Usar a caixa de pesquisa do QnA Maker acima das perguntas e respostas para reduzir a exibição apenas para itens correspondentes ao filtro](../media/qnamaker-how-to-edit-kb/search-paginate-group.png)

* **Paginação**: Mover rapidamente pelas fontes de dados para gerenciar grandes bases de dados de conhecimento

    ![Usar os recursos de paginação do QnA Maker acima das perguntas e respostas para percorrer as páginas de perguntas e respostas](../media/qnamaker-how-to-edit-kb/pagination.png)

## <a name="delete-knowledge-bases"></a>Excluir as bases de dados de conhecimento

Excluir uma base de conhecimento (KB) é uma operação permanente. Não pode ser desfeito. Antes de excluir uma base de conhecimento, você deverá exportar a base de dados de conhecimento da página **Configurações** do portal do QnA Maker. 

Se você compartilhar seu KB com [colaboradores](collaborate-knowledge-base.md) e então o excluir, todos perderão acesso ao KB. 

## <a name="delete-azure-resources"></a>Excluir recursos do Azure 

Se você excluir qualquer um dos recursos do Azure usados em suas bases de conhecimento do QnA Maker, as bases de conhecimento não funcionarão mais. Antes de excluir qualquer recurso, exporte suas bases de conhecimento da página **Configurações**. 

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Colaborar em uma base de dados de conhecimento](./collaborate-knowledge-base.md)
