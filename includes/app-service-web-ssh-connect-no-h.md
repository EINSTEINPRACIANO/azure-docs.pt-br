---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/29/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 060bc1039982cc0a77214d5dbe2a08de7a839c84
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66138222"
---
Para abrir uma sessão SSH direta com seu contêiner, seu aplicativo deve estar em execução.

Cole a seguinte URL no seu navegador e substitua \<app_name> pelo nome do aplicativo:

```
https://<app-name>.scm.azurewebsites.net/webssh/host
```

Se você ainda não estiver autenticado, será necessário autenticar com sua assinatura do Azure para se conectar. Uma vez autenticado, consulte um shell no navegador, onde você pode executar comandos dentro de seu contêiner.

![Conexão SSH](./media/app-service-web-ssh-connect-no-h/app-service-linux-ssh-connection.png)
