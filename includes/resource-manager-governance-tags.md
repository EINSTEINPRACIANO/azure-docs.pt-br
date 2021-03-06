---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 11/20/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c426df2293cfb2d8ba4dc02e8fc5519c3d822168
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66128896"
---
Você pode aplicar marcas aos recursos do Azure, fornecendo metadados para organizá-los logicamente em uma taxonomia. Cada marca consiste em um par de nome/valor. Por exemplo, você pode aplicar o nome "Ambiente" e o valor "Produção" a todos os recursos na produção.

Depois de aplicar marcas, você pode recuperar todos os recursos em sua assinatura com esse nome e valor de marca. As marcas permitem que você recupere recursos relacionados de diferentes grupos de recursos. Esta abordagem será útil quando você precisar organizar os recursos de gerenciamento ou de cobrança.

Sua taxonomia deve considerar uma estratégia de marcação de metadados de autoatendimento, além de uma estratégia de marcação automática para reduzir a carga nos usuários e aumentar a precisão.

As seguintes limitações se aplicam a marcas:

* Nem todos os tipos de recursos suportam tags. Para determinar se você pode aplicar uma tag a um tipo de recurso, consulte [Suporte a tags para recursos do Azure](../articles/azure-resource-manager/tag-support.md).
* Cada recurso ou grupo de recursos pode ter um máximo de 15 pares de nome/valor de marca. Essa limitação se aplica somente a marcas aplicadas diretamente ao grupo de recursos ou recurso. Um grupo de recursos pode conter muitos recursos que possuem 15 pares de nome/valor de marca. Se você tiver mais de 15 valores que você precisa associar a um recurso, use uma cadeia de caracteres JSON para o valor da marca. A cadeia de caracteres JSON pode conter diversos valores que são aplicados a um único nome de marca. Este artigo mostra um exemplo de atribuição de uma cadeia de caracteres JSON para a marca.
* O nome da marca é limitado a 512 caracteres e o valor da marca é limitado a 256 caracteres. Para contas de armazenamento, o nome da marca é limitado a 128 caracteres e o valor da marca é limitado a 256 caracteres.
* As máquinas virtuais e conjuntos de dimensionamento de máquina Virtual são limitados a um total de 2048 caracteres para todos os valores e nomes de marca.
* Marcas aplicadas ao grupo de recursos não são herdadas pelos recursos desse grupo de recursos.
* As marcas não podem ser aplicadas a recursos clássicos como Serviços de Nuvem.
* Os nomes das marcas não podem conter esses caracteres: `<`, `>`, `%`, `&`, `\`, `?`, `/`
