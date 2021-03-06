---
title: Conceitos ‑ Azure e terminologia de serviços de mídia do Azure | Microsoft Docs
description: Este tópico fornece uma visão geral dos conceitos e terminologia de serviços de mídia do Azure e fornece links para obter mais detalhes.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/13/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 1e76569c7f5157dce681d15ec8d499b90e080102
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65762300"
---
# <a name="media-services-concepts"></a>Conceitos de serviços de mídia

Este tópico fornece uma visão geral dos conceitos e terminologia de serviços de mídia do Azure. O artigo também fornece links para artigos com a explicação detalhada sobre os serviços de mídia v3 conceitos e funcionalidade. 

Os conceitos fundamentais descritos nestes tópicos devem ser examinados antes do início do desenvolvimento.

> [!NOTE]
> Atualmente, você não pode usar o portal do Azure para gerenciar recursos da v3. Use a [API REST](https://aka.ms/ams-v3-rest-ref), a [CLI](https://aka.ms/ams-v3-cli-ref) ou um dos [SDKs](media-services-apis-overview.md#sdks) com suporte.

## <a name="terminology"></a>Terminologia

Esta seção mostra como alguns termos comuns do setor são mapeados para a API dos serviços de mídia v3.

### <a name="live-event"></a>Evento ao Vivo

Um **evento ao vivo** representa um pipeline para ingestão, transcodificação (opcionalmente) e transmissões ao vivo de metadados de vídeo, áudio e em tempo real de empacotamento.

Migrando de APIs do serviços de mídia v2, o **evento ao vivo** substitui a **canal** entidade no v2. Para obter mais informações, consulte [Migrando do v2 para v3](migrate-from-v2-to-v3.md).

### <a name="streaming-endpoint-packaging-and-origin"></a>Ponto de extremidade Streaming (empacotamento e a origem)

Um **ponto de extremidade de Streaming** representa um dinâmico (just-in-time) empacotamento e a origem serviço que pode entregar seu conteúdo ao vivo e sob demanda diretamente a um aplicativo de player de cliente, usando um dos protocolos de mídia streaming (HLS comuns ou DASH). Além disso, o **ponto de extremidade de Streaming** fornece a criptografia dinâmica (just-in-time) para DRMs de líderes do setor.

Na mídia de streaming do setor, esse serviço é conhecido como um **Packager** ou **origem**.  Outros termos comuns do setor para essa funcionalidade incluem JITP (Just-no-tempo-packager) ou JITE (Just-no-tempo-criptografia). 
 
## <a name="cloud-upload-and-storage"></a>Upload e armazenamento na nuvem

Para começar a gerenciar, criptografar, codificação, analisar e streaming de conteúdo de mídia no Azure, você precisa criar uma conta de serviços de mídia e carregar seus arquivos digitais em **ativos**.

- [Upload e armazenamento na nuvem](storage-account-concept.md)
- [Conceito de ativos](assets-concept.md)

## <a name="encoding"></a>Codificação

Depois de carregar arquivos de mídia digital de alta qualidade em ativos, você poderá codificá-los em formatos que podem ser executados em uma ampla variedade de navegadores e dispositivos. 

Para codificar com os serviços de mídia v3, você precisará criar **transforma** e **trabalhos**.

![Transformações](./media/encoding/transforms-jobs.png)

- [Transformações e Trabalhos](transforms-jobs-concept.md)
- [Codificação com os serviços de mídia](encoding-concept.md)

## <a name="media-analytics"></a>Análise de Mídia

Para analisar seus arquivos de áudio e vídeos, você também precisará criar **transforma** e **trabalhos**.

- [Analisando os arquivos de áudio e vídeos](analyzing-video-audio-files-concept.md)

## <a name="packaging-delivery-protection"></a>Empacotamento, entrega, proteção

Depois que seu conteúdo é codificado, você pode aproveitar **empacotamento dinâmico**. Nos serviços de mídia, uma **ponto de extremidade de Streaming**  /origem é o serviço de empacotamento dinâmico usado para distribuir o conteúdo de mídia para jogadores do cliente. Para disponibilizar vídeos no ativo de saída para os clientes de reprodução, você precisa criar uma **localizador de Streaming** e, em seguida, criar URLs de streaming. 

Ao criar o **localizador de Streaming**, além do nome do ativo, você precisará especificar **Streaming política**. **Políticas de streaming** permitem que você definir os protocolos de streaming e opções de criptografia (se houver) para seus **localizadores de Streaming**.

Empacotamento dinâmico é usado se você transmitir seu conteúdo ao vivo ou sob demanda. O diagrama a seguir mostra o streaming sob demanda com o fluxo de trabalho de empacotamento dinâmico.

![Empacotamento dinâmico](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

Com os serviços de mídia, você pode entregar seu conteúdo ao vivo e sob demanda criptografado dinamicamente com criptografia AES (AES-128) ou / e qualquer um dos três sistemas DRM (gerenciamento) principais de direitos digitais: Microsoft PlayReady, Google Widevine e Apple FairPlay. Os serviços de mídia também fornecem um serviço de distribuição de chaves AES e licenças DRM (PlayReady, Widevine e FairPlay) para os clientes autorizados.

Se especificar opções de criptografia em seu fluxo, crie o **política de chave de conteúdo** e associá-la com seus **localizador de Streaming**. O **política de chave de conteúdo** permite que você configure como a chave de conteúdo é entregue para os clientes finais.

A seguinte imagem ilustra o fluxo de trabalho de proteção de conteúdo dos Serviços de Mídia: 

![Proteger conteúdo](./media/content-protection/content-protection.svg)

&#42;criptografia dinâmica dá suporte a chave AES-128"Limpar", CBCS e CENC. 

Você pode usar os serviços de mídia **manifestos dinâmico** transmitir apenas uma representação específica ou subclipes do vídeo. No exemplo a seguir, o codificador foi usado para codificar um ativo mezzanine em sete representações de vídeo ISO MP4s (de 180p para 1080p). O recurso codificado pode ser empacotado dinamicamente em qualquer um dos seguintes protocolos de streaming: HLS, MPEG DASH e Smooth.  Na parte superior do diagrama, é mostrado o manifesto HLS para o ativo sem filtros (ele contém todas as sete representações).  Na parte inferior esquerda, é mostrado o manifesto HLS ao qual foi aplicado um filtro chamado "ott". O filtro "ott" especifica a remoção de todas as taxas de bits abaixo de 1 Mbps, o que resultou na remoção dos dois níveis de qualidade inferiores na resposta. Na parte inferior direita, é mostrado o manifesto HLS, ao qual foi aplicado um filtro chamado "mobile". O filtro "mobile" especifica a remoção de representações em que a resolução é maior do que 720p, resultando na remoção de duas representações de 1080p.

![Filtragem de representação](./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png)

- [Empacotamento dinâmico](dynamic-packaging-overview.md)
- [Pontos de Extremidade de Streaming](streaming-endpoint-concept.md)
- [Localizadores de Streaming](streaming-locators-concept.md)
- [Políticas de Streaming](streaming-policy-concept.md)
- [Políticas de Chave de Conteúdo](content-key-policy-concept.md)
- [Proteção de conteúdo](content-protection-overview.md)
- [Manifestos dinâmicos](filters-dynamic-manifest-overview.md)
- [Filtros](filters-concept.md)

## <a name="live-streaming"></a>Transmissão ao vivo

O Azure Media Services permite entregar eventos ao vivo para seus clientes na nuvem do Azure. **Eventos ao Vivo** são responsáveis pela ingestão e pelo processamento dos feeds de vídeo ao vivo. Quando você cria um **evento ao vivo**, um ponto de extremidade de entrada é criado que você pode usar para enviar um sinal ao vivo de um codificador remoto. Uma vez que o fluxo está fluindo para o **evento ao vivo**, você pode começar o evento de transmissão criando um **ativo**, **Live saída**, e **localizador de Streaming** . **Live saída** arquivará a transmitir para o **ativo** e torná-lo disponível para visualizadores por meio de **ponto de extremidade de Streaming**. Um **evento ao vivo** pode ser um dos dois tipos: **passagem** e **codificação ativa**.

A imagem a seguir ilustra o fluxo de trabalho do tipo de passagem:

![passagem](./media/live-streaming/pass-through.svg)

- [Visão geral da transmissão ao vivo](live-streaming-overview.md)
- [Eventos ao Vivo e Saídas Dinâmicas](live-events-outputs-concept.md)

## <a name="monitoring"></a>Monitoramento

### <a name="event-grid"></a>Grade de Eventos

Para ver o andamento do trabalho, você deve usar **grade de eventos**. Os serviços de mídia também emite os tipos de evento ao vivo. Com a Grade de Eventos, seus aplicativos podem escutar e reagir a eventos de praticamente todos os serviços do Azure, bem como de origens personalizadas. 

- [Manipulação de eventos da grade de eventos](reacting-to-media-services-events.md)
- [Schemas](media-services-event-schemas.md)

### <a name="azure-monitor"></a>Azure Monitor

Monitorar métricas e logs de diagnóstico que ajudam você a entendem como os seus aplicativos são executados com o Azure Monitor.

- [As métricas e logs de diagnóstico](media-services-metrics-diagnostic-logs.md)
- [Esquemas de logs de diagnóstico](media-services-diagnostic-logs-schema.md)

## <a name="player-clients"></a>Clientes do Player

Você pode usar o Player de mídia do Azure para reproduzir conteúdo de mídia transmitido pelos serviços de mídia em uma grande variedade de navegadores e dispositivos. O Player de Mídia do Azure usa padrões do setor, como HTML5, MSE (Media Source Extensions) e EME (Encrypted Media Extensions) para fornecer uma experiência avançada de streaming adaptável. 

- [Visão geral do Player de Mídia do Azure](use-azure-media-player.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Fazer perguntas, comentar, obter atualizações

Confira o artigo [comunidade dos Serviços de Mídia do Azure](media-services-community.md) para ver diferentes maneiras de fazer perguntas, comentários e obter atualizações sobre os serviços de mídia.

## <a name="next-steps"></a>Próximas etapas

* [Codificar o arquivo remoto e o fluxo de vídeo – REST](stream-files-tutorial-with-rest.md)
* [Codificar o arquivo carregado e o fluxo de vídeo – .NET](stream-files-tutorial-with-api.md)
* [Transmitir ao vivo – .NET](stream-live-tutorial-with-api.md)
* [Analisar vídeo – .NET](analyze-videos-tutorial-with-api.md)
* [Criptografia dinâmica AES-128 – .NET](protect-with-aes128.md)
* [Criptografar dinamicamente com vários DRM – .NET](protect-with-drm.md) 
