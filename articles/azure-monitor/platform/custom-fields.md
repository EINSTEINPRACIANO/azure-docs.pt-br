---
title: Campos personalizados no Azure Monitor | Microsoft Docs
description: O recurso de campos personalizados do Azure Monitor permite que você crie seus próprios campos pesquisáveis de registros em um espaço de trabalho do Log Analytics que são adicionados às propriedades de um registro coletado.  Este artigo descreve o processo para criar um campo personalizado e fornece um passo a passo detalhado com um evento de exemplo.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 31572b51-6b57-4945-8208-ecfc3b5304fc
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2019
ms.author: bwren
ms.openlocfilehash: 974a3391c592a1caf7bdcc6d9e01032f0c73aaa6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61461688"
---
# <a name="create-custom-fields-in-a-log-analytics-workspace-in-azure-monitor"></a>Criar campos personalizados em um espaço de trabalho do Log Analytics no Azure Monitor

> [!NOTE]
> Este artigo descreve como analisar dados de texto em um espaço de trabalho do Log Analytics conforme ele é coletado. Há vantagens para análise de dados de texto em uma consulta depois de coletadas conforme descrito em [analisar dados de texto no Azure Monitor](../log-query/parse-text.md).

O **campos personalizados** recurso do Azure Monitor permite que você estenda os registros existentes no seu espaço de trabalho do Log Analytics adicionando seus próprios campos pesquisáveis.  Os campos personalizados são populados automaticamente por meio dos dados extraídos de outras propriedades no mesmo registro.

![Visão geral](media/custom-fields/overview.png)

Por exemplo, o registro de exemplo abaixo tem dados úteis escondidos na descrição do evento. Extrair esses dados em uma propriedade separada torna disponíveis para ações como a classificação e filtragem.

![Extração de exemplo](media/custom-fields/sample-extract.png)

> [!NOTE]
> Na Preview, você está limitado a 100 campos personalizados em seu workspace.  Esse limite será expandido quando essa funcionalidade atingir a disponibilidade geral.

## <a name="creating-a-custom-field"></a>Criando um campo personalizado
Quando você cria um campo personalizado, o Log Analytics deve compreender quais dados deseja usar para popular seu valor.  Ele usa uma tecnologia da Microsoft Research chamada FlashExtract para identificar rapidamente esses dados.  Em vez de exigir que você forneça instruções explícitas, Monitor do Azure aprende sobre os dados que você deseja extrair de exemplos que você fornecer.

As seções a seguir fornecem o procedimento para criar um campo personalizado.  Na parte inferior deste artigo há um passo a passo de uma extração de exemplo.

> [!NOTE]
> O campo personalizado é populado conforme os registros correspondentes aos critérios especificados são adicionados ao espaço de trabalho do Log Analytics, portanto, ele só serão exibidos nos registros coletados depois que o campo personalizado é criado.  O campo personalizado não será adicionado aos registros que já estão no armazenamento de dados quando ele for criado.
> 

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Etapa 1: Identificar registros que terão o campo personalizado
A primeira etapa é identificar os registros que obterão o campo personalizado.  Começar com uma [consulta de log padrão](../log-query/log-query-overview.md) e, em seguida, selecione um registro para atuar como o modelo que orientará Azure Monitor de.  Quando você especifica que extrairá dados em um campo personalizado, o **Field Extraction Wizard** (Assistente de Extração de Campo) é aberto, no qual você valida e refina os critérios.

1. Vá para **Logs** e usar um [consulta para recuperar os registros](../log-query/log-query-overview.md) que farão com que o campo personalizado.
2. Selecione um registro que o Log Analytics usará para atuar como um modelo para extrair dados para popular o campo personalizado.  Você identificará os dados que deseja extrair desse registro e o Log Analytics usará essas informações para determinar a lógica para popular o campo personalizado para todos os registros semelhantes.
3. Expanda as propriedades de registro, clique na elipse à esquerda da propriedade superior do registro e selecione **extrair campos de**.
4. O **Assistente de extração de campo** é aberto, e o registro selecionado é exibido na **exemplo principal** coluna.  O campo personalizado será definido para os registros com os mesmos valores nas propriedades selecionadas.  
5. Se a seleção não for exatamente o que você deseja, selecione campos adicionais para restringir os critérios.  Para alterar os valores de campo para os critérios, você deve cancelar e selecionar um registro diferente que corresponde aos critérios desejados.

### <a name="step-2---perform-initial-extract"></a>Etapa 2: Executar a extração inicial.
Depois de identificar os registros que terão o campo personalizado, você identifica os dados que deseja extrair.  O Log Analytics usará essas informações para identificar padrões semelhantes em registros semelhantes.  Na próxima etapa, você poderá validar os resultados e fornecer mais detalhes para o Log Analytics usar em suas análises.

1. Realce o texto no registro de exemplo que você deseja para popular o campo personalizado.  Em seguida, você verá uma caixa de diálogo para fornecer um nome e tipo de dados para o campo e para realizar a extração inicial.  Os caracteres **\_CF** serão acrescentados automaticamente.
2. Clique em **Extrair** para executar uma análise dos registros coletados.  
3. As seções **Resumo** e **Resultados da Pesquisa** exibem os resultados da extração para que você possa inspecionar sua precisão.  **Resumo** exibe os critérios usados para identificar registros e uma contagem de cada um dos valores de dados identificados.  **Resultados da Pesquisa** fornece uma lista detalhada dos registros que correspondem aos critérios.

### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Etapa 3: Verificar a precisão da extração e criar o campo personalizado
Após você ter executado a extração inicial, o Log Analytics exibirá seus resultados com base nos dados que já foram coletados.  Se os resultados parecerem precisos, você poderá criar o campo personalizado sem trabalho adicional.  Caso contrário, você pode refinar os resultados para que o Log Analytics possa melhorar sua lógica.

1. Se quaisquer valores na extração inicial não estiverem corretos, clique no ícone **Editar** ao lado de um registro impreciso e selecione **Modificar esse realce** para modificar a seleção.
2. A entrada é copiada para a seção **Exemplos adicionais** sob o **Exemplo Principal**.  Você pode ajustar o realce aqui para ajudar o Log Analytics a entender a seleção que ele deveria ter feito.
3. Clique em **Extrair** para usar essas novas informações para avaliar todos os registros existentes.  Os resultados podem ser modificados para registros diferentes daqueles que você acabou de modificar com base nessa nova inteligência.
4. Continue a adicionar correções até que todos os registros na extração identifiquem corretamente os dados para popular o novo campo personalizado.
5. Clique em **Save Extract** (Salvar Extração) quando estiver satisfeito com os resultados.  O campo personalizado agora está definido, mas ele não será adicionado a nenhum registro ainda.
6. Aguarde até que novos registros correspondentes aos critérios especificados sejam coletados e, em seguida, execute novamente a pesquisa de log. Os novos registros devem ter o campo personalizado.
7. Use o campo personalizado como qualquer outra propriedade de registro.  Você pode usá-lo para agregar e agrupar dados e até mesmo usá-lo para gerar novas percepções.

## <a name="viewing-custom-fields"></a>Exibindo campos personalizados
Exiba uma lista de todos os campos personalizados do grupo de gerenciamento no menu **Configurações Avançadas** do espaço de trabalho do Log Analytics no portal do Azure.  Selecione **Dados** e **Campos personalizados** para obter uma lista de todos os campos personalizados no workspace.  

![Campos Personalizados](media/custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Removendo um campo personalizado
Há duas maneiras de remover um campo personalizado.  O primeiro é a opção **Remover** para cada campo ao exibir a lista completa conforme descrito acima.  O outro método é recuperar um registro e clicar no botão à esquerda do campo.  O menu terá uma opção para remover o campo personalizado.

## <a name="sample-walkthrough"></a>Passo a passo de exemplo
A seção a seguir explica passo a passo um exemplo completo de criação de campo personalizado.  Este exemplo extrai o nome do serviço em eventos do Windows que indicam um serviço alterando o estado.  Isso se baseia em eventos criados pelo Gerenciador de controle de serviço durante a inicialização do sistema em computadores Windows.  Se você deseja acompanhar este exemplo, deve estar [coletando eventos de informações para o log do sistema](data-sources-windows-events.md).

Inserimos a seguinte consulta para retornar todos os eventos do Gerenciador de Controle de Serviço que têm uma ID de Evento de 7036, que é o evento que indica um serviço iniciando ou parando.

![Consultar](media/custom-fields/query.png)

Podemos, em seguida, selecione e expanda qualquer registro com ID de evento 7036.

![Registro de origem](media/custom-fields/source-record.png)

Podemos definir campos personalizados clicando-se a elipse ao lado da propriedade superior.

![Extrair campos](media/custom-fields/extract-fields.png)

O **Field Extraction Wizard** (Assistente de Extração de Campo) é aberto e os campos **EventLog** e **EventID** são selecionados na coluna **Main Example** (Exemplo Principal).  Isso indica que o campo personalizado será definido para eventos do log do sistema com uma ID de evento de 7036.  Isso é suficiente para que não precisemos selecionar nenhum outro campo.

![Main Example](media/custom-fields/main-example.png)

Realçamos o nome do serviço na propriedade **RenderedDescription** e usamos **Service** para identificar o nome do serviço.  O campo personalizado será chamado **Service_CF**. Nesse caso, o tipo de campo é uma cadeia de caracteres, portanto, podemos deixar que inalterado.

![Título do campo](media/custom-fields/field-title.png)

Vemos que o nome do serviço é identificado corretamente para alguns registros, mas não para outros.   Os **Resultados da Pesquisa** mostram que parte do nome do **Adaptador de Desempenho WMI** não foi selecionada.  O **resumo** mostra que um registro identificado **instalador de módulos** em vez de **instalador de módulos do Windows**.  

![Resultados da Pesquisa](media/custom-fields/search-results-01.png)

Começamos com o registro **Adaptador de Desempenho WMI** .  Clicamos em seu ícone de edição e em **Modify this highlight**(Modificar esse realce).  

![Modificar o realce](media/custom-fields/modify-highlight.png)

Aumentaremos o realce para incluir a palavra **WMI** e, em seguida, executamos a extração novamente.  

![Exemplo adicional](media/custom-fields/additional-example-01.png)

Podemos ver que as entradas para **Adaptador de Desempenho WMI** foram corrigidas e o Log Analytics também usou essa informação para corrigir os registros para **Instalador de Módulo do Windows**.

![Resultados da Pesquisa](media/custom-fields/search-results-02.png)

Agora, podemos executar uma consulta que verifica **Service_CF** é criado, mas ainda não foi adicionado a nenhum registro. Isso ocorre porque o campo personalizado não funciona em registros existentes, portanto, precisamos esperar para novos registros a serem coletados.

![Contagem inicial](media/custom-fields/initial-count.png)

Depois de passar algum tempo para que novos eventos sejam coletados, podemos ver que o campo **Service_CF** agora está sendo adicionado a recursos que correspondem aos nossos critérios.

![Resultados finais](media/custom-fields/final-results.png)

Agora podemos usar o campo personalizado como qualquer outra propriedade de registro.  Para ilustrar isso, criamos uma consulta que agrupa pelo novo campo **Service_CF** para inspecionar quais serviços são os mais ativos.

![Agrupar por consulta](media/custom-fields/query-group.png)

## <a name="next-steps"></a>Próximas etapas
* Saiba mais sobre [registrar consultas](../log-query/log-query-overview.md) para criar consultas usando campos personalizados para os critérios.
* Monitorar [arquivos de log personalizados](data-sources-custom-logs.md) que você analisa usando campos personalizados.

