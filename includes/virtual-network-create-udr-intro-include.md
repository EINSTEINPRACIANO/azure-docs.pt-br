---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 482241deb1081ac8a5265a076eabbdc3fb6d659e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66170882"
---
Embora o uso de rotas do sistema facilite o tráfego automaticamente para a sua implantação, há casos em que você deseja controlar o roteamento de pacotes por meio de um dispositivo virtual. Você pode fazer isso criando rotas definidas pelo usuário que especificam o próximo salto para os pacotes que fluem para uma sub-rede específica indo, então, para o dispositivo virtual e habilitar o encaminhamento de IP para a VM em execução como o dispositivo virtual.

Alguns dos casos nos quais os dispositivos virtuais podem ser usados incluem:

* Monitorando o tráfego com um sistema de detecção de intrusões (IDS)
* Controlando o tráfego com um firewall

Para saber mais sobre UDR, visite [Rotas definidas pelo usuário e encaminhamento de IP](../articles/virtual-network/virtual-networks-udr-overview.md).

