---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 21dbec52dded5a195cc764741ab9e8d79737c549
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121168"
---
1. Inicie sessão na sua assinatura do Azure com o comando [az login](/cli/azure/) e siga as instruções na tela. Para obter mais informações sobre como conectar-se, confira [Introdução à CLI do Azure](/cli/azure/get-started-with-azure-cli).

   ```azurecli
   az login
   ```
2. Se tiver mais de uma assinatura do Azure, liste as assinaturas para a conta.

   ```azurecli
   az account list --all
   ```
3. Especifique a assinatura que você deseja usar.

   ```azurecli
   az account set --subscription <replace_with_your_subscription_id>
   ```
