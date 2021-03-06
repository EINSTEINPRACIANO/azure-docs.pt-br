---
title: Entender mensagens de nuvem para o dispositivo Hub IoT do Azure| Microsoft Docs
description: Guia do desenvolvedor - como usar mensagens de nuvem para o dispositivo com o Hub IoT. Inclui informações sobre o ciclo de vida da mensagem e as opções de configuração.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: b0c1b877a9468ce9c3b851bce62cb87c64c04260
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472731"
---
# <a name="send-cloud-to-device-messages-from-iot-hub"></a>Enviar mensagens de nuvem para o dispositivo do Hub IoT

Para enviar notificações unidirecionais para o aplicativo do dispositivo do back-end de solução, envie mensagens de nuvem para o dispositivo do seu Hub IoT para seu dispositivo. Para uma discussão sobre outras opções de nuvem para dispositivo com suporte do Hub IoT, consulte [Diretrizes de comunicação de nuvem para dispositivo](iot-hub-devguide-c2d-guidance.md).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Você pode enviar mensagens da nuvem para o dispositivo por meio de um ponto de extremidade voltado para o serviço (**/messages/devicebound**). Um dispositivo recebe as mensagens através de um ponto de extremidade específico do dispositivo (**/devices/{deviceId}/messages/devicebound**).

Para que cada mensagem da nuvem para o dispositivo seja direcionada a um único dispositivo, o Hub IoT define a propriedade **to** como **/devices/{deviceId}/messages/devicebound**.

Cada fila de dispositivo contém no máximo 50 mensagens da nuvem para o dispositivo. Tentar enviar mais mensagens para o mesmo dispositivo resultará em um erro.

## <a name="the-cloud-to-device-message-lifecycle"></a>O ciclo de vida da mensagem de nuvem para o dispositivo

Para garantir uma entrega de mensagem pelo menos uma vez, o Hub IoT persiste com mensagens de nuvem para o dispositivo em filas por dispositivo. Os dispositivos devem reconhecer explicitamente a *conclusão* para que o Hub IoT as remova da fila. Essa abordagem garante a resiliência contra conectividade e falhas de dispositivo.

O diagrama a seguir mostra o grafo de estado do ciclo de vida para uma mensagem de nuvem para o dispositivo no Hub IoT.

![Ciclo de vida de mensagens da nuvem para o dispositivo](./media/iot-hub-devguide-messages-c2d/lifecycle.png)

Quando o serviço do Hub IoT envia uma mensagem para um dispositivo, o serviço define o estado da mensagem para **Enfileirada**. Quando um dispositivo deseja *receber* uma mensagem, o Hub IoT *bloqueia* a mensagem (definindo o estado para **Invisível**), permitindo que outros threads no dispositivo comecem a receber outras mensagens. Quando um thread de dispositivo conclui o processamento de uma mensagem, ele notifica o Hub IoT *concluindo* a mensagem. O Hub IoT, em seguida, define o estado para **Concluído**.

Um dispositivo também pode escolher:

* *Rejeitar* a mensagem, fazendo com que o Hub IoT defina-a para o estado **Mensagens mortas**. Os dispositivos que se conectam através do protocolo MQTT não podem rejeitar mensagens de nuvem para o dispositivo.

* *Abandonar* a mensagem, o que faz com que o Hub IoT coloque a mensagem de volta na fila com o estado definido como **Enfileirada**. Os dispositivos que se conectam através do protocolo MQTT não podem abandonar mensagens de nuvem para o dispositivo.

Um thread pode falhar ao processar uma mensagem sem notificar o Hub IoT. Nesse caso, as mensagens passam automaticamente do estado **Invisível** de volta para o estado **Enfileirada** após um *tempo limite de visibilidade (ou de bloqueio)*. O valor padrão desse tempo limite é um minuto.

A propriedade **max delivery count** no Hub IoT determina o número de vezes que uma mensagem pode transitar entre os estados **Enfileirada** e **Invisível**. Após esse número de transições, o Hub IoT define o estado da mensagem para **Mensagens mortas**. Da mesma forma, o Hub IoT define o estado de uma mensagem para **Mensagens mortas** após o tempo de expiração (consulte [Vida útil](#message-expiration-time-to-live)).

O artigo [Como enviar mensagens de nuvem para dispositivo com Hub IoT](iot-hub-csharp-csharp-c2d.md) mostra como enviar mensagens de nuvem para dispositivo da nuvem e recebê-las em um dispositivo.

Normalmente, um dispositivo completa uma mensagem de nuvem para o dispositivo quando a perda da mensagem não afeta a lógica de aplicativo. Por exemplo, quando o dispositivo persistiu o conteúdo da mensagem localmente ou executou uma operação com êxito. A mensagem também poderia portar informações transitórias cuja perda não afetaria a funcionalidade do aplicativo. Às vezes, para tarefas de execução longa, você pode:

* Completar a mensagem do dispositivo para a nuvem depois de manter a descrição da tarefa no armazenamento local.

* Notificar o back-end da solução com uma ou mais mensagens de dispositivo para a nuvem em vários estágios de progresso da tarefa.

## <a name="message-expiration-time-to-live"></a>Expiração da mensagem (vida útil)

Todas as mensagens da nuvem para o dispositivo têm um tempo de expiração. Esse tempo é definido por um dos seguintes:

* A propriedade **ExpiryTimeUtc** no serviço.
* O Hub IoT usando a *vida útil* padrão especificada como uma propriedade do Hub IoT.

Consulte [Opções de configuração da nuvem para o dispositivo](#cloud-to-device-configuration-options).

Uma maneira comum de tirar proveito da expiração da mensagem e evitar enviar mensagens para dispositivos desconectados é definir valores de vida útil baixos. Essa abordagem proporciona o mesmo resultado que a manutenção do estado de conexão do dispositivo, enquanto é mais eficiente. Quando você solicita confirmações de mensagem, o Hub IoT notifica quais dispositivos:

* São capazes de receber mensagens.
* Não estão online ou falharam.

## <a name="message-feedback"></a>Comentários da mensagem

Quando você envia uma mensagem da nuvem para o dispositivo, o serviço pode solicitar a entrega de um comentário por mensagem sobre o estado final dessa mensagem. Isso é feito definindo o `iothub-ack` propriedade do aplicativo na mensagem C2D sendo enviada para qualquer um dos seguintes valores:

| Valor da propriedade ACK | Comportamento |
| ------------ | -------- |
| **nenhum**     | O IoT Hub não gera uma mensagem de comentários (comportamento padrão). |
| **positivo** | Se a mensagem da nuvem para o dispositivo alcança o estado **Concluído**, o Hub IoT gera uma mensagem de comentários. |
| **negativo** | Se a mensagem de nuvem para dispositivo alcançar o estado **Mensagens mortas**, o Hub IoT gerará uma mensagem de comentários. |
| **full**     | O Hub IoT gera uma mensagem de comentários em ambos os casos. |

Se **Ack** estiver definida como **total** e você não receber uma mensagem de comentários, isso significará que a mensagem de comentários expirou. O serviço não pode saber o que aconteceu com a mensagem original. Na prática, um serviço deve garantir que possa processar os comentários antes que eles expirem. O tempo de expiração máximo é de dois dias, tempo suficiente para restabelecer a execução do serviço caso ocorra uma falha.

Como explicado em [Pontos de extremidade](iot-hub-devguide-endpoints.md), o Hub IoT fornece comentários por meio de um ponto de extremidade voltado para o serviço (**/messages/servicebound/feedback**) como mensagens. A semântica de recebimento dos comentários é a mesma das mensagens da nuvem para o dispositivo. Sempre que possível, os comentários de mensagem são feitos em lotes em uma única mensagem, com o seguinte formato:

| Propriedade     | Descrição |
| ------------ | ----------- |
| EnqueuedTime | O carimbo de hora que indica quando a mensagem de comentários foi recebida pelo hub. |
| UserId       | `{iot hub name}` |
| ContentType  | `application/vnd.microsoft.iothub.feedback.json` |

O corpo é uma matriz de registros serializada em JSON, cada um com as seguintes propriedades:

| Propriedade           | Descrição |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | Carimbo de data e hora que indica quando ocorreu a saída da mensagem. Por exemplo, o hub recebeu a mensagem de comentários ou a mensagem original expirou. |
| OriginalMessageId  | **MessageId** da mensagem de nuvem para o dispositivo para qual essa informação de comentários se relaciona. |
| StatusCode         | Cadeia de caracteres obrigatória. Usado em mensagens de comentários geradas pelo Hub IoT. <br/> 'Êxito' <br/> 'Expirado' <br/> 'DeliveryCountExceeded' <br/> 'Rejeitado' <br/> 'Limpo' |
| Descrição        | Valores de cadeia de caracteres para **StatusCode**. |
| deviceId           | **DeviceId** do dispositivo de destino da mensagem de nuvem para o dispositivo para qual esse comentário se relaciona. |
| DeviceGenerationId | **DeviceGenerationId** do dispositivo de destino da mensagem de nuvem para o dispositivo para qual esse comentário se relaciona. |

O serviço deve especificar uma **MessageId** para a mensagem da nuvem para o dispositivo, para poder correlacionar seus comentários com a mensagem original.

O exemplo a seguir mostra o corpo de uma mensagem de comentários.

```json
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0,
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

## <a name="cloud-to-device-configuration-options"></a>Opções de configuração da nuvem para o dispositivo

Cada Hub IoT expõe as seguintes opções de configuração para mensagens de nuvem para o dispositivo:

| Propriedade                  | Descrição | Intervalo e padrão |
| ------------------------- | ----------- | ----------------- |
| defaultTtlAsIso8601       | TTL padrão para mensagens da nuvem para o dispositivo. | Intervalo ISO_8601 de até 2D (mínimo de 1 minuto). Padrão: 1 hora. |
| maxDeliveryCount          | Contagem máxima de entrega para filas de nuvem para o dispositivo por dispositivo. | 1 a 100. Padrão: 10. |
| feedback.ttlAsIso8601     | Retenção de mensagens informativas do serviço associado. | Intervalo ISO_8601 de até 2D (mínimo de 1 minuto). Padrão: 1 hora. |
| feedback.maxDeliveryCount |Contagem máxima de entrega para a fila de comentários. | 1 a 100. Padrão: 100. |

Para obter mais informações sobre como configurar essas opções de configuração, consulte [Criar Hubs IoT](iot-hub-create-through-portal.md).

## <a name="next-steps"></a>Próximas etapas

Para obter informações sobre os SDKs que você pode utilizar para receber mensagens de nuvem para dispositivo, consulte [SDKs do IoT do Azure](iot-hub-devguide-sdks.md).

Para experimentar a recepção de mensagens de nuvem para dispositivo, consulte o tutorial [Enviar de nuvem para dispositivo ](iot-hub-csharp-csharp-c2d.md).