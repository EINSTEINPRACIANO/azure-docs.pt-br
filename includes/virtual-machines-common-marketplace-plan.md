---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 4d2235eaea457c89d01a632afa5dd5a862bec344
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66117309"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Implantar uma imagem com termos do Marketplace

Algumas imagens de VM no Azure Marketplace têm licenças e termos de compra adicionais que você deve aceitar antes de implantá-las programaticamente.  

Para implantar uma VM por meio de uma dessas imagens, você precisará aceitar os termos da imagem e habilitar a implantação programática. Será necessário fazer isso apenas uma vez por assinatura. Posteriormente, todas as vezes que você implantar uma VM programaticamente a partir da imagem, também será necessário especificar os parâmetros do *plano de compra*.

As seções a seguir mostram como:

* Descubra se uma imagem do Marketplace tem termos de licença adicionais 
* Aceitar os termos de forma programática
* Fornecer parâmetros de plano de compra quando você implantar uma VM de forma programática

