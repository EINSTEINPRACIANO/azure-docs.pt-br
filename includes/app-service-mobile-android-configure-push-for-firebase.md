---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 51a75ee7bf87c38e3916bdbc8d85abcfb14dca8b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140268"
---
1. No [portal do Azure](https://portal.azure.com/), clique em **Procurar Tudo** > **Serviços de Aplicativos** e clique em seu back-end de Aplicativos Móveis. Em **Configurações**, clique em **Push do Serviço de Aplicativo** e clique no nome do hub de notificação.
2. Vá para **Google (GCM)**, insira o valor **Chave do Servidor** obtido do Firebase no procedimento anterior e clique em **Salvar**.

    ![Definir a chave de API no portal](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

O back-end de Aplicativos Móveis agora está configurado para usar o Firebase Cloud Messaging. Isso permite que você envie notificações por push para seu aplicativo em execução em um dispositivo Android usando o hub de notificação.
