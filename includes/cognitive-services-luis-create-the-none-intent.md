---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 355fe134939b26c51d6e03368f782845628a6b96
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66123750"
---
O aplicativo cliente precisa saber se um enunciado não é significativo ou apropriado para o aplicativo. A intenção **None** é adicionada a cada aplicativo como parte do processo de criação para determinar se um enunciado não pode ser atendido pelo aplicativo cliente.

Se o LUIS retornar a intenção **None** para um enunciado, o aplicativo cliente poderá perguntar se o usuário deseja terminar a conversa ou fornecer mais orientações para continuar a conversa. 

> [!CAUTION] 
> Não deixe a intenção **Nenhum** vazia. 

1. Selecione **Intenções** no painel esquerdo.

2. Selecione a intenção **None**. Adicione três enunciados que o usuário possa inserir, mas que não sejam relevantes para o aplicativo de recursos humanos:

    | Exemplo de enunciados|
    |--|
    |Cachorros que latem demais são irritantes|
    |Peça uma pizza para mim|
    |Pinguins no oceano|
