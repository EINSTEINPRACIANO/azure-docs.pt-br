---
title: Atualizar a configuração de um cluster do Service Fabric do Azure | Microsoft Docs
description: Saiba como atualizar a configuração que executa um cluster do Service Fabric no Azure usando um modelo do Resource Manager.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/09/2018
ms.author: dekapur
ms.openlocfilehash: 77b9b20f99f00ef87c4907c2890cb3a21d20ec75
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62096259"
---
# <a name="upgrade-the-configuration-of-a-cluster-in-azure"></a>Atualizar a configuração de um cluster no Azure 

Este artigo descreve como personalizar as várias configurações de malha e a política de atualização para o cluster do Service Fabric. Para clusters hospedados no Azure, você pode personalizá-los através do [portal do Azure](https://portal.azure.com) ou utilizando um modelo do Azure Resource Manager.

> [!NOTE]
> Nem todas as configurações estão disponíveis no portal e é um [práticas recomendadas para personalizá-lo usando um modelo do Azure Resource Manager](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code); Portal é para o Service Fabric Dev\Test do cenário apenas.
> 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Personalize as configurações do cluster, utilizando os modelos do Resource Manager
Clusters do Azure podem ser configurados por meio do modelo do Resource Manager do JSON. Para saber mais sobre as diferentes configurações, consulte [Definições de configuração para clusters](service-fabric-cluster-fabric-settings.md). Por exemplo, as etapas a seguir mostram como adicionar uma nova configuração *MaxDiskQuotaInMB* à seção *diagnóstico* usando o Gerenciador de recursos do Azure.

1. Acesse https://resources.azure.com
2. Navegue até sua assinatura expandindo **assinaturas** -> **\<Sua assinatura >** -> **resourceGroups** -> **\<Seu grupo de recursos>** -> **provedores** -> **Microsoft.ServiceFabric** -> **clusters** -> **\<Nome do seu cluster>**
3. No canto superior direito, selecione **Leitura/Gravação.**
4. Selecione **Editar** e atualize o elemento `fabricSettings` JSON e adicione um novo elemento:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Você também pode personalizar as configurações de cluster em uma das maneiras a seguir com o Azure Resource Manager:

- Use o [portal do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template) para exportar e atualizar o modelo do Gerenciador de Recursos.
- Use o [PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-powershell) para exportar e atualizar o modelo do Gerenciador de Recursos.
- Use a [CLI do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template-cli) para exportar e atualizar o modelo do Gerenciador de Recursos.
- Usar o Azure PowerShell [Set-AzServiceFabricSetting](https://docs.microsoft.com/powershell/module/az.servicefabric/Set-azServiceFabricSetting) e [AzServiceFabricSetting remover](https://docs.microsoft.com/powershell/module/az.servicefabric/Remove-azServiceFabricSetting) comandos para modificar a configuração diretamente.
- Usar os comandos de configuração do cluster[az sf da CLI do Azure](https://docs.microsoft.com/cli/azure/sf/cluster/setting) para modificar a configuração diretamente.

## <a name="next-steps"></a>Próximas etapas
* Saiba mais sobre [configurações de cluster do Service Fabric](service-fabric-cluster-fabric-settings.md).
* Saiba como [reduzir e escalar horizontalmente seu cluster](service-fabric-cluster-scale-up-down.md).
