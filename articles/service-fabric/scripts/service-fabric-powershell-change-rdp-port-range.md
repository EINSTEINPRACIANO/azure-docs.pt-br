---
title: Exemplo de Script do PowerShell do Azure - Alterar o intervalo de porta do RDP | Microsoft Docs
description: Exemplo de Script do Microsoft Azure PowerShell - Altera o intervalo de porta do RDP de um cluster implantado.
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 03/19/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 001574cef8f0ea074b65fdc94f6d8b255f98bff5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66159959"
---
# <a name="update-the-rdp-port-range-values"></a>Atualizar os valores de intervalo da porta RDP

Esse exemplo de script altera os valores de intervalo de porta do RDP nas VMs do nó de cluster após a implantação do cluster.  O Azure PowerShell é usado para que as VMs subjacentes não se movimentem ciclicamente.  O script obtém o recurso `Microsoft.Network/loadBalancers` no grupo de recursos do cluster e atualiza os valores `inboundNatPools.frontendPortRangeStart` e `inboundNatPools.frontendPortRangeEnd`. Personalize os parâmetros conforme necessário.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Se necessário, instale o Azure PowerShell usando a instrução encontrada no [guia do Azure PowerShell](/powershell/azure/overview). 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os seguintes comandos. Cada comando da tabela é vinculado à documentação específica do comando.

| Comando | Observações |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Obtém o recurso `Microsoft.Network/loadBalancers`. |
|[Set-AzResource](/powershell/module/az.resources/set-azresource)|Atualiza o recurso `Microsoft.Network/loadBalancers`.|

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre o módulo do Azure PowerShell, confira [Documentação do Azure PowerShell](/powershell/azure/overview).

Mais exemplos do Azure PowerShell para o Azure Service Fabric podem ser encontrados nos [exemplos do Azure PowerShell](../service-fabric-powershell-samples.md).
