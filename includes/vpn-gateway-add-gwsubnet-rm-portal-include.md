---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/30/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: eb2555cdc163ee0f88149248b9e7d83a51d34a1b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66150255"
---
1. No [portal do Azure](http://portal.azure.com), selecione a Rede Virtual do Resource Manager para a qual você quer criar um gateway de rede virtual.

2. Na seção **Configurações** seção da página de rede virtual, selecione **Sub-redes** para expandir a página **Sub-redes**.

3. Na página **Sub-redes**, selecione **Sub-rede de gateway** para abrir a página **Adicionar sub-rede**.

   ![Adicionar a sub-rede de gateway](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsub.png "Adicionar a sub-rede de gateway")

4. O **Nome** da sub-rede será automaticamente preenchido com o valor *GatewaySubnet*. Esse valor é necessário para o Azure reconhecer a sub-rede como a sub-rede do gateway. Ajuste os valores do **Intervalo de endereços** para corresponder aos requisitos de configuração e, em seguida, selecione **OK** para criar a sub-rede.

   ![Adicionar a sub-rede](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addsubnetgw.png "Adicionar sub-rede")