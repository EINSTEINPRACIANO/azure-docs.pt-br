---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: dc3fe8375a7f9b43bc866b3f8364bc9b2acb4e50
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66138911"
---
Crie um [aplicativo Web](../articles/app-service/containers/app-service-linux-intro.md) no plano do Serviço de Aplicativo `myAppServicePlan`. 

No Cloud Shell, é possível usar o comando [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest). No exemplo a seguir, substitua `<app-name>` por um nome do aplicativo exclusivo globalmente (os caracteres válidos são `a-z`, `0-9` e `-`). A execução é predefinida para `NODE|6.9`. Para ver todos os tempos de execução com suporte, execute [`az webapp list-runtimes`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-list-runtimes). 

```azurecli-interactive
# Bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "NODE|6.9" --deployment-local-git
# PowerShell
az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "NODE|6.9" --deployment-local-git
```

Quando o aplicativo Web for criado, a CLI do Azure mostrará um resultado semelhante ao seguinte exemplo:

```json
Local git is configured with url of 'https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Você criou um aplicativo Web vazio com a implantação do git habilitada.

> [!NOTE]
> A URL do Git remoto é mostrada na propriedade `deploymentLocalGitUrl` com o formato `https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git`. Salve essa URL, pois você precisará dela mais tarde.
>
