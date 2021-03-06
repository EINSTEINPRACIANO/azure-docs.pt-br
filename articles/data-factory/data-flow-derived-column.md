---
title: Transformação de coluna derivada de mapeamento de fluxo de dados do Azure Data Factory
description: Como transformar dados em grande escala com o Azure Data Factory Mapeando dados fluxo de transformação coluna derivada
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: 6568e5ebf356bb0e6b4ac8ff6059cd093f8da821
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64917575"
---
# <a name="mapping-data-flow-derived-column-transformation"></a>Mapeamento de fluxo de dados de transformação de coluna derivada

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Use a transformação de Coluna Derivada para gerar novas colunas em seu fluxo de dados ou modificar campos existentes.

![derivar coluna](media/data-flow/dc1.png "Coluna Derivada")

Você pode executar várias ações de Coluna Derivada em uma única transformação de Coluna Derivada. Clique em "Adicionar coluna" para transformar mais de uma coluna na etapa única de transformação.

No campo Coluna, selecione uma coluna existente para sobrepor com um novo valor derivado ou clique em "Criar Nova Coluna" para gerar uma nova coluna com o novo valor derivado.

A caixa de texto Expressão abrirá o Construtor de Expressões onde você poderá criar a expressão para as colunas derivadas usando funções de expressão.

## <a name="column-patterns"></a>Padrões de coluna

Se os nomes de coluna são variáveis de suas fontes, você poderá criar transformações dentro da coluna derivada usando padrões de coluna em vez de usar colunas nomeadas. Consulte a [descompasso do esquema](concepts-data-flow-schema-drift.md) artigo para obter mais detalhes.

![padrão de coluna](media/data-flow/columnpattern.png "padrões de coluna")

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre o [linguagem de expressão de Data Factory para transformações](https://aka.ms/dataflowexpressions) e o [construtor de expressões](concepts-data-flow-expression-builder.md)
