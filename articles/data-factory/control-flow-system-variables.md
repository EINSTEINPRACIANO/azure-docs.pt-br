---
title: Variáveis do sistema no Azure Data Factory | Microsoft Docs
description: Este artigo descreve as variáveis do sistema com suporte pelo Azure Data Factory. Você pode usar essas variáveis em expressões ao definir entidades do Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: shlo
ms.openlocfilehash: 93a83545699e3536eb0045d538225d01cd1a96a2
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235650"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Variáveis do sistema com suporte pelo Azure Data Factory
Este artigo descreve as variáveis do sistema com suporte pelo Azure Data Factory. Você pode usar essas variáveis em expressões ao definir entidades do Data Factory.

## <a name="pipeline-scope"></a>Escopo do pipeline
Essas variáveis de sistema podem ser referenciadas em qualquer lugar no pipeline dp JSON.

| Nome da Variável | Descrição |
| --- | --- |
| @pipeline().DataFactory |Nome do data factory em que a execução do pipeline está ocorrendo |
| @pipeline().Pipeline |Nome do pipeline |
| @pipeline().RunId | ID da execução do pipeline específica |
| @pipeline().TriggerType | Tipo de gatilho que invocou o pipeline (Manual, Agendador) |
| @pipeline().TriggerId| ID do gatilho que invoca o pipeline |
| @pipeline().TriggerName| Nome do gatilho que invoca o pipeline |
| @pipeline().TriggerTime| Hora em que o gatilho invocou o pipeline. A hora do gatilho é a hora do disparo real, não a hora agendada. Por exemplo, `13:20:08.0149599Z` é retornado em vez de `13:20:00.00Z` |

## <a name="schedule-trigger-scope"></a>Escopo do Gatilho de Agendamento
Essas variáveis de sistema podem ser mencionadas em qualquer lugar no JSON do gatilho se este for do tipo: "ScheduleTrigger".

| Nome da Variável | Descrição |
| --- | --- |
| @trigger().scheduledTime |Hora em que o gatilho foi agendado para invocar a execução do pipeline. Por exemplo, para um gatilho disparado a cada cinco minutos, essa variável retornaria `2017-06-01T22:20:00Z`, `2017-06-01T22:25:00Z` e `2017-06-01T22:29:00Z` respectivamente.|
| @trigger().startTime |Hora em que o gatilho **realmente** foi disparado para invocar a execução do pipeline. Por exemplo, para um gatilho disparado a cada cinco minutos, essa variável retornaria algo como `2017-06-01T22:20:00.4061448Z`, `2017-06-01T22:25:00.7958577Z` e `2017-06-01T22:29:00.9935483Z` respectivamente. (Observação: O carimbo de hora é por padrão no formato ISO 8601)|

## <a name="tumbling-window-trigger-scope"></a>Escopo do Gatilho de Janela em Cascata
Essas variáveis de sistema podem ser mencionadas em qualquer lugar no JSON do gatilho se este for do tipo: "TumblingWindowTrigger".
(Observação: O carimbo de hora é por padrão no formato ISO 8601)

| Nome da Variável | Descrição |
| --- | --- |
| @trigger().outputs.windowStartTime |Início da janela em que o gatilho foi agendado para invocar a execução do pipeline. Se o gatilho da janela em cascata tiver uma frequência "por hora", esse seria o horário no início da hora.|
| @trigger().outputs.windowEndTime |Fim da janela em que o gatilho foi agendado para invocar a execução do pipeline. Se o gatilho da janela em cascata tiver uma frequência "por hora", esse seria o horário no fim da hora.|
## <a name="next-steps"></a>Próximas etapas
Para obter informações sobre como essas variáveis são usadas em expressões, consulte [Funções e linguagem de expressão](control-flow-expression-language-functions.md).
