---
title: Mensagens mortas e nova tentativa de políticas para as assinaturas da Grade de Eventos do Azure
description: Descreve como personalizar opções de entrega de eventos para a Grade de Eventos. Definir um destino de inatividade e especificar o tempo de entrega novamente.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/06/2019
ms.author: spelluru
ms.openlocfilehash: a1b49fd3a2a85377a56c92aefd1b0056f91895b1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66119572"
---
# <a name="dead-letter-and-retry-policies"></a>Mensagens mortas e tentar novas políticas

Ao criar uma assinatura de evento, você pode personalizar as configurações para entrega de eventos. Este artigo mostra como configurar um local de mensagens mortas e personalizar as configurações de repetição. Para obter informações sobre esses recursos, consulte [Entrega e repetição de mensagens da Grade de Eventos](delivery-and-retry.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="set-dead-letter-location"></a>Defina o local de mensagens mortas

Para definir um local de mensagens mortas, é necessário ter uma conta de armazenamento para armazenar eventos que não podem ser entregues a um ponto de extremidade. Os exemplos obtêm a ID de recurso de uma conta de armazenamento existente. Eles criam uma assinatura de evento que usa um contêiner na conta de armazenamento para o ponto de extremidade de inatividade.

> [!NOTE]
> Crie uma conta de armazenamento e um contêiner de blob no armazenamento antes de executar os comandos neste artigo.

### <a name="azure-cli"></a>CLI do Azure

```azurecli-interactive
containername=testcontainer

topicid=$(az eventgrid topic show --name demoTopic -g gridResourceGroup --query id --output tsv)
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Para desativar a colocação em fila de mensagens mortas, execute novamente o comando para criar a assinatura de evento, mas não forneça um valor para `deadletter-endpoint`. Você não precisa excluir a assinatura do evento.

> [!NOTE]
> Se você estiver usando a CLI do Azure em seu computador local, use a CLI do Azure versão 2.0.56 ou superior. Para obter instruções sobre como instalar a versão mais recente da CLI do Azure, confira [Instalar a CLI do Azure](/cli/azure/install-azure-cli).

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
$containername = "testcontainer"

$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id
$storageid = (Get-AzStorageAccount -ResourceGroupName gridResourceGroup -Name demostorage).Id

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeadLetterEndpoint "$storageid/blobServices/default/containers/$containername"
```

Para desativar a colocação em fila de mensagens mortas, execute novamente o comando para criar a assinatura de evento, mas não forneça um valor para `DeadLetterEndpoint`. Você não precisa excluir a assinatura do evento.

> [!NOTE]
> Se você estiver usando o Azure PowerShell em seu computador local, use o Azure PowerShell versão 1.1.0 ou superior. Baixe e instale o Azure PowerShell mais recente de [Downloads do Azure](https://azure.microsoft.com/downloads/).

## <a name="set-retry-policy"></a>Definir a política de repetição

Ao criar uma assinatura de Grade de Eventos, você pode definir valores de por quanto tempo a Grade de Eventos deve tentar entregar o evento. Por padrão, a Grade de Eventos tenta por 24 horas (1440 minutos) ou 30 vezes. Você pode definir esses valores para a sua assinatura de grade de eventos. O valor de tempo de vida do evento deve ser um inteiro de 1 a 1440. O valor para máximo de tentativas deve ser um inteiro de 1 a 30.

Não é possível configurar um [plano de tentativas](delivery-and-retry.md#retry-schedule-and-duration).

### <a name="azure-cli"></a>CLI do Azure

Para definir o evento de vida útil para um valor diferente de 1440 minutos, use:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --event-ttl 720
```

Para definir o máximo de tentativas com um valor diferente de 30, use:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Se você definir ambos `event-ttl` e `max-deliver-attempts`, a Grade de Eventos usa o primeiro para expirar, a fim de determinar quando parar a entrega de eventos.

### <a name="powershell"></a>PowerShell

Para definir o evento de vida útil para um valor diferente de 1440 minutos, use:

```azurepowershell-interactive
$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -EventTtl 720
```

Para definir o máximo de tentativas com um valor diferente de 30, use:

```azurepowershell-interactive
$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -MaxDeliveryAttempt 18
```

Se você definir ambos `EventTtl` e `MaxDeliveryAttempt`, a Grade de Eventos usa o primeiro para expirar, a fim de determinar quando parar a entrega de eventos.

## <a name="next-steps"></a>Próximas etapas

* Para um aplicativo de exemplo que usa um aplicativo de função do Azure para processar eventos de mensagens mortas, consulte [Exemplos de Mensagens Mortas de Grade de Eventos do Azure para .NET](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/).
* Para obter informações sobre a entrega de eventos e repetições, [Entrega e repetição de mensagens da Grade de Eventos](delivery-and-retry.md).
* Para ver uma introdução à Grade de Eventos, confira [About Event Grid](overview.md) (Sobre a Grade de Eventos).
* Para começar a usar rapidamente a Grade de Eventos, confira [Create and route custom events with Azure Event Grid](custom-event-quickstart.md) (Criar e rotear eventos personalizados com a Grade de Eventos do Azure).
