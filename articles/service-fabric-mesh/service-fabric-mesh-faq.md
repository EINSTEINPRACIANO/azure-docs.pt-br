---
title: Perguntas comuns sobre Malha do Microsoft Azure Service Fabric | Microsoft Docs
description: Saiba mais sobre perguntas frequentes e respostas relacionadas à Malha do Microsoft Azure Service Fabric.
services: service-fabric-mesh
keywords: ''
author: chackdan
ms.author: chackdan
ms.date: 4/23/2019
ms.topic: troubleshooting
ms.service: service-fabric-mesh
manager: jeanpaul.connock
ms.openlocfilehash: 950f9ac89b9d3224db29b32fe2d1e403ccc98116
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143285"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Perguntas frequentes sobre Malha do Service Fabric

A Malha do Microsoft Azure Service Fabric é um serviço totalmente gerenciado que permite aos desenvolvedores implantar aplicativos de microsserviços sem gerenciar máquinas virtuais, armazenamento ou rede. Este artigo apresenta as respostas das perguntas frequentes.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>Como fazer para relatar um problema ou fazer uma pergunta?

Faça perguntas, obtenha respostas de engenheiros da Microsoft e relate problemas no [Repositório GitHub de versão prévia da Malha do Service Fabric ](https://aka.ms/sfmeshissues).

## <a name="quota-and-cost"></a>Cota e custo

### <a name="what-is-the-cost-of-participating-in-the-preview"></a>Qual é o custo de participar da versão prévia?

Atualmente, não há encargos para a implantação de aplicativos ou contêineres para a visualização de malha. Assista para atualizações em maio para a habilitação de cobrança. No entanto, é recomendável que você exclua os recursos que você implanta e não deixá-los em execução, a menos que você está testando-os ativamente.

### <a name="is-there-a-quota-limit-of-the-number-of-cores-and-ram"></a>Há um limite de cota do número de núcleos e RAM?

Sim. As cotas de cada assinatura são:

- Número de aplicativos: 5
- Núcleos por aplicativo: 12
- Total de RAM por aplicativo: 48 GB
- Pontos de extremidade de Entrada e Rede: 5
- Volumes do Azure possíveis de anexar: 10
- Número de réplicas de serviço: 3
- O maior contêiner que pode ser implantado é limitado a 4 núcleos e 16 GB de RAM.
- É possível alocar núcleos parciais para os contêineres em incrementos de 0,5 núcleos até um máximo de 6 núcleos.

### <a name="how-long-can-i-leave-my-application-deployed"></a>Por quanto tempo posso deixar meu aplicativo implantado?

Atualmente, limitamos o tempo de vida de um aplicativo a dois dias. Isso é para maximizar o uso dos núcleos livres alocados para a versão prévia. Como resultado, você só tem permissão para executar uma determinada implantação continuamente por 48 horas; depois disso, ela será desligada.

Se isso acontecer, você poderá validar que o sistema a desliga executando o comando `az mesh app show` na CLI do Azure . Verifique se ele retorna `"status": "Failed", "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue."` 

Por exemplo:  

```cli
~$ az mesh app show --resource-group myResourceGroup --name helloWorldApp
{
  "debugParams": null,
  "description": "Service Fabric Mesh HelloWorld Application!",
  "diagnostics": null,
  "healthState": "Ok",
  "id": "/subscriptions/1134234-b756-4979-84re-09d671c0c345/resourcegroups/myResourceGroup/providers/Microsoft.ServiceFabricMesh/applications/helloWorldApp",
  "location": "eastus",
  "name": "helloWorldApp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "serviceNames": [
    "helloWorldService"
  ],
  "services": null,
  "status": "Failed",
  "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue.",
  "tags": {},
  "type": "Microsoft.ServiceFabricMesh/applications",
  "unhealthyEvaluation": null
}
```

Para excluir o grupo de recursos, use o comando `az group delete <nameOfResourceGroup>`.

## <a name="deployments"></a>Implantações

### <a name="what-container-images-are-supported"></a>Quais imagens de contêiner têm suporte?

Se você estiver desenvolvendo em um computador com Windows Fall Creators Update (versão 1709), somente poderá usar as imagens do docker da versão 1709 do Windows.

Se estiver desenvolvendo em um computador com a atualização de 10 de abril de 2018 do Windows (versão 1803), você poderá usar imagens do Docker do Windows versão 1709 ou do Windows versão 1803.

As imagens do SO do contêiner a seguir podem ser usadas para implantar serviços:

- Windows - windowsservercore e nanoserver
    - Windows Server 1709
    - Windows Server 1803
    - Windows Server 1809
    - Windows Server 2019 LTSC
- Linux
    - Sem limitações conhecidas

> [!NOTE]
> Ferramentas para a malha do Visual Studio ainda não suporta implantação no Windows Server 2019 e 1809 contêineres.

### <a name="what-types-of-applications-can-i-deploy"></a>Quais tipos de aplicativos pode implantar? 

Você pode implantar qualquer coisa que é executado em contêineres que se ajustam as restrições colocados em um recurso de aplicativo (consulte acima para obter mais informações sobre cotas). Se detectarmos que você estiver usando a malha para executar cargas de trabalho ilegais ou abuso de sistema (ou seja, de mineração), em seguida, nos reservamos o direito para finalizar suas implantações e a lista de bloqueios sua assinatura em execução no serviço. Entre em contato conosco se tiver alguma dúvida sobre como executar uma carga de trabalho específica. 

## <a name="developer-experience-issues"></a>Problemas de experiência do desenvolvedor

### <a name="dns-resolution-from-a-container-doesnt-work"></a>A resolução de DNS de um contêiner não funciona

As consultas DNS de saída de um contêiner para o serviço DNS do Service Fabric poderão falhar em determinadas circunstâncias. Isso está sendo investigado. Para atenuar:

- Use o Windows Fall Creators update (versão 1709) ou superior como sua imagem de contêiner base.
- Se o nome do serviço sozinho não funcionar, tente o nome totalmente qualificado: ServiceName.ApplicationName.
- No arquivo do Docker para seu serviço, adicionar `EXPOSE <port>` onde a porta é a porta na qual você está expondo seu serviço. Por exemplo: 

```Dockerfile
EXPOSE 80
```

### <a name="dns-does-not-work-the-same-as-it-does-for-service-fabric-development-clusters-and-in-mesh"></a>O DNS não funciona da mesma maneira que nos clusters de desenvolvimento do Service Fabric e na Malha

Talvez seja necessário referenciar serviços de maneira diferente no cluster de desenvolvimento local e na Malha do Azure.

Use `{serviceName}.{applicationName}` no seu cluster de desenvolvimento local. Na Malha do Azure Service Fabric, use `{servicename}`. 

A Malha do Azure não dá suporte à resolução de DNS nos aplicativos.

Para outros problemas conhecidos de DNS com a execução de um cluster de desenvolvimento do Service Fabric no Windows 10, consulte: [Depurar contêineres do Windows](/azure/service-fabric/service-fabric-how-to-debug-windows-containers) e [problemas de DNS conhecidos](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice#known-issues).

### <a name="networking"></a>Rede

A rede NAT do ServiceFabric pode desaparecer durante o uso e execução do seu aplicativo em seu computador local. Para realizar um diagnóstico se isso acontecer, execute o seguinte em um prompt de comando:

`docker network ls` e observe se `servicefabric_nat` está listado.  Caso contrário, execute o seguinte comando: `docker network create -d=nat --subnet 10.128.0.0/24 --gateway 10.128.0.1 servicefabric_nat`

Isso solucionará o problema, mesmo se o aplicativo já estiver implantado localmente e em um estado não íntegro.

### <a name="issues-running-multiple-apps"></a>Problemas ao executar vários aplicativos

Você pode encontrar a disponibilidade de CPU e os limites que estão sendo corrigidos em todos os aplicativos. Para atenuar:
- Crie um cluster de cinco nós.
- Reduza o uso da CPU em serviços do aplicativo que é implantado. Por exemplo, no arquivo de service.yaml do seu serviço, altere `cpu: 1.0` para `cpu: 0.5`

Vários aplicativos não podem ser implantados em um cluster de um nó. Para atenuar:
- Use um cluster de cinco nós durante a implantação de vários aplicativos em um cluster local.
- Remova os aplicativos que não estão sendo testados.

### <a name="vs-tooling-has-limited-support-for-windows-containers"></a>Ferramentas do VS tem suporte limitado para contêineres do Windows

As ferramentas do Visual Studio dá suporte apenas a implantação de contêineres do Windows com uma versão de sistema operacional base do Windows Server 1709 e 1803 de hoje. 

## <a name="feature-gaps-and-other-known-issues"></a>Falhas de recurso e outros problemas conhecidos

### <a name="after-deploying-my-application-the-network-resource-associated-with-it-does-not-have-an-ip-address"></a>Após implantar o aplicativo, o recurso de rede associado ao aplicativo tem um endereço IP

Há um problema conhecido em que o endereço IP não se torna disponível imediatamente. Verifique o status do recurso de rede em alguns minutos para ver o endereço IP associado.

### <a name="my-application-fails-to-access-the-right-networkvolume-resource"></a>O aplicativo não está conseguindo acessar o recurso de rede/volume correto

No modelo de aplicativo, use a ID do recurso completa de redes e volumes para poder acessar o recurso associado. Veja um exemplo do início rápido:

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

### <a name="when-i-scale-out-all-of-my-containers-are-affected-including-running-ones"></a>Ao escalar horizontalmente, todos os contêineres são afetados, incluindo aqueles em execução

Este é um bug e uma correção está sendo implementada.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre a Malha do Service Fabric, leia a [visão geral](service-fabric-mesh-overview.md).
