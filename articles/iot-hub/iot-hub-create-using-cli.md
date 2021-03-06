---
title: Criar um Hub IoT usando a CLI do Azure | Microsoft Docs
description: Como criar um Hub IoT do Azure usando a CLI do Azure.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: robinsh
ms.openlocfilehash: 78ea9071f220b2a78c6d9260d47145f22284d760
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66166280"
---
# <a name="create-an-iot-hub-using-the-azure-cli"></a>Criar um Hub IoT usando a CLI do Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Este artigo mostra como criar um Hub IoT usando a CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir estas instruções, você precisa de uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-and-set-your-azure-account"></a>Entre e configure sua conta do Azure

Se você estiver executando a CLI do Azure localmente, em vez de usar o Cloud Shell, você precisará entrar em sua conta do Azure.

No prompt de comando, execute o [comando de logon](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli):

   ```azurecli
   az login
   ```

Siga as instruções de autenticação usando o código e entre em sua conta do Azure por meio de um navegador da Web.

## <a name="create-an-iot-hub"></a>Crie um Hub IoT

Use a CLI do Azure para criar um grupo de recursos e, em seguida, adicione um Hub IoT.

1. Quando cria um Hub IoT, você deve criá-lo em um grupo de recursos. Use um grupo de recursos existente ou execute o seguinte [comando para criar um grupo de recursos](https://docs.microsoft.com/cli/azure/resource):
    
   ```azurecli
   az group create --name {your resource group name} --location westus
   ```

   > [!TIP]
   > O exemplo anterior cria o grupo de recursos localizado no Oeste dos EUA. Você pode exibir uma lista dos locais disponíveis executando o comando: 
   >
   >``` bash
   >az account list-locations -o table
   >```
   >

2. Execute o seguinte [comando para criar um Hub IoT](https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-create) no seu grupo de recursos, usando um nome globalmente exclusivo para o Hub IoT:
    
   ```azurecli
   az iot hub create --name {your iot hub name} \
      --resource-group {your resource group name} --sku S1
   ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


O comando anterior cria um Hub IoT com tipo de preço S1, pelo qual você será cobrado. Para saber mais, confira [Preço do Hub IoT do Azure](https://azure.microsoft.com/pricing/details/iot-hub/).

## <a name="remove-an-iot-hub"></a>Remover um Hub IoT

Você pode usar a CLI do Azure para [excluir um recurso individual](https://docs.microsoft.com/cli/azure/resource), tal como um Hub IoT, ou excluir um grupo de recursos e todos os seus recursos, incluindo quaisquer Hubs IoT.

Para [excluir um Hub IoT](https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-delete), execute o seguinte comando:

```azurecli
az iot hub delete --name {your iot hub name} -\
  -resource-group {your resource group name}
```

Para [excluir um grupo de recursos](https://docs.microsoft.com/cli/azure/group#az-group-delete) e todos os seus recursos, execute o seguinte comando:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre como usar um Hub IoT, veja os seguintes artigos:

* [Guia do desenvolvedor do Hub IoT](iot-hub-devguide.md)
* [Usar o portal do Azure para gerenciar o Hub IoT](iot-hub-create-through-portal.md)
