---
title: Atualizar e reiniciar nós do Linux com kured no serviço de Kubernetes do Azure (AKS)
description: Saiba como atualizar nós do Linux e reinicializá-las automaticamente com o kured no serviço de Kubernetes do Azure (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 02/28/2019
ms.author: iainfou
ms.openlocfilehash: 1702d9558e27452006a2f015fd3312ac19362871
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65849862"
---
# <a name="apply-security-and-kernel-updates-to-linux-nodes-in-azure-kubernetes-service-aks"></a>Aplicar segurança e atualizações de kernel para nós do Linux no serviço de Kubernetes do Azure (AKS)

Para proteger seus clusters, atualizações de segurança são aplicadas automaticamente a nós do Linux no AKS. Essas atualizações incluem correções de segurança do SO ou atualizações de kernel. Algumas dessas atualizações exigem uma reinicialização do nó para concluir o processo. AKS não reinicializar automaticamente esses nós do Linux para concluir o processo de atualização.

O processo para manter nós do Windows Server (atualmente em visualização no AKS) atualizado é um pouco diferente. Nós do Windows Server não recebem atualizações diárias. Em vez disso, você executa uma atualização AKS que implanta os novos nós com os patches e a imagem base mais recente do Windows Server. Para clusters AKS que usam nós do Windows Server, consulte [Upgrade de um pool de nós no AKS][nodepool-upgrade].

Este artigo mostra como usar o código-fonte aberto [kured (Daemon de reinicialização do KUbernetes)] [ kured] assistir para nós do Linux que exigem uma reinicialização, em seguida, tratar automaticamente o reagendamento da execução de pods e nó Reinicie o processo.

> [!NOTE]
> `Kured` é um projeto de software livre da Weaveworks. O suporte para esse projeto no AKS é fornecido com base no melhor esforço. Suporte adicional pode ser encontrado no canal do slack #weave-community.

## <a name="before-you-begin"></a>Antes de começar

Este artigo considera que já existe um cluster do AKS. Se você precisar de um cluster do AKS, confira o guia de início rápido do AKS [Usando a CLI do Azure][aks-quickstart-cli] ou [Usando o portal do Azure][aks-quickstart-portal].

Você também precisa da CLI do Azure versão 2.0.59 ou posterior instalado e configurado. Execute  `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, confira  [Instalar a CLI do Azure][install-azure-cli].

## <a name="understand-the-aks-node-update-experience"></a>Compreender a experiência de atualização do nó AKS

Em um cluster do AKS, os nós do Kubernetes executam como VMs (máquinas virtuais) do Azure. Essas VMs baseadas em Linux usam uma imagem do Ubuntu, com o SO configurado para verificar automaticamente as atualizações todas as noites. Se atualizações de kernel ou de segurança estiverem disponíveis, elas serão baixadas e instaladas automaticamente.

![Atualização de nó do AKS e processo de reinicialização com kured](media/node-updates-kured/node-reboot-process.png)

Algumas atualizações de segurança, como atualizações de kernel, exigem uma reinicialização do nó para finalizar o processo. Um nó do Linux que requer uma reinicialização cria um arquivo chamado */var/run/reboot-required*. Esse processo de reinicialização não ocorre automaticamente.

Você pode usar seus próprios fluxos de trabalho e processos para manipular reinicializações de nós ou usar `kured` para orquestrar o processo. Com o `kured`, um [DaemonSet] [ DaemonSet] é implantado que executa um pod em cada nó no cluster. Esses pods no DaemonSet inspecionam a existência do arquivo */var/run/reboot-required* e, em seguida, iniciam um processo para reinicializar os nós.

### <a name="node-upgrades"></a>Upgrades de nós

Há um processo adicional no AKS que permite fazer *upgrade* de um cluster. Um upgrade é basicamente passar para uma versão mais nova do Kubernetes, e não apenas aplicar atualizações de segurança do nó. Um upgrade de AKS executa as seguintes ações:

* Um novo nó é implantado com as atualizações de segurança mais recentes e a versão do Kubernetes aplicada.
* Um nó antigo é isolado e drenado.
* Os pods estão agendados no novo nó.
* O nó antigo é excluído.

Não será possível permanecer na mesma versão do Kubernetes durante um evento de upgrade. Você deverá especificar uma versão mais nova do Kubernetes. Para fazer upgrade para a última versão do Kubernetes, você pode [fazer upgrade do seu cluster do AKS][aks-upgrade].

## <a name="deploy-kured-in-an-aks-cluster"></a>Implantar em um cluster do AKS

Para implantar o DaemonSet `kured`, aplique o seguinte exemplo de manifesto YAML da sua página de projeto do GitHub. Esse manifesto cria uma função e uma função de cluster, associações e uma conta de serviço e, em seguida, implanta o DaemonSet usando a versão 1.1.0 de `kured` que dá suporte aos clusters do AKS 1.9 ou posterior.

```console
kubectl apply -f https://github.com/weaveworks/kured/releases/download/1.2.0/kured-1.2.0-dockerhub.yaml

You can also configure additional parameters for `kured`, such as integration with Prometheus or Slack. For more information about additional configuration parameters, see the [kured installation docs][kured-install].

## Update cluster nodes

By default, Linux nodes in AKS check for updates every evening. If you don't want to wait, you can manually perform an update to check that `kured` runs correctly. First, follow the steps to [SSH to one of your AKS nodes][aks-ssh]. Once you have an SSH connection to the Linux node, check for updates and apply them as follows:

```console
sudo apt-get update && sudo apt-get upgrade -y
```

Se forem aplicadas atualizações que exigem uma reinicialização do nó, um arquivo será gravado em */var/run/reboot-required*. `Kured` verifica por nós que exigem uma reinicialização a cada 60 minutos por padrão.

## <a name="monitor-and-review-reboot-process"></a>Monitorar e revisar o processo de reinicialização

Quando uma das réplicas no DaemonSet detectou que uma reinicialização do nó é necessária, um bloqueio foi colocado no nó por meio da API do Kubernetes. Esse bloqueio impedirá que pods adicionais sejam agendados no nó. O bloqueio também indica que apenas um nó deverá ser reinicializado de cada vez. Com o nó isolado, os pods em execução serão drenados do nó e o nó será reinicializado.

É possível monitorar o status dos nós usando o comando [kubectl get nodes][kubectl-get-nodes]. A seguinte saída de exemplo mostra um nó com um status *SchedulingDisabled*, na medida em que o nó se prepara para o processo de reinicialização:

```
NAME                       STATUS                     ROLES     AGE       VERSION
aks-nodepool1-28993262-0   Ready,SchedulingDisabled   agent     1h        v1.11.7
```

Quando o processo de atualização estiver concluído, você poderá exibir o status dos nós usando o comando [kubectl get nodes][kubectl-get-nodes] com o parâmetro `--output wide`. Essa saída adicional permite ver uma diferença em *KERNEL-VERSION* dos nós subjacentes, conforme mostrado na seguinte saída de exemplo. O *aks-nodepool1-28993262-0* foi atualizado em uma etapa anterior e mostra a versão kernel *4.15.0-1039-azure*. O nó *aks-nodepool1-28993262-1* que ainda não foi atualizada mostra a versão de kernel *4.15.0-1037-azure*.

```
NAME                       STATUS    ROLES     AGE       VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-28993262-0   Ready     agent     1h        v1.11.7   10.240.0.4    <none>        Ubuntu 16.04.6 LTS   4.15.0-1039-azure   docker://3.0.4
aks-nodepool1-28993262-1   Ready     agent     1h        v1.11.7   10.240.0.5    <none>        Ubuntu 16.04.6 LTS   4.15.0-1037-azure   docker://3.0.4
```

## <a name="next-steps"></a>Próximas etapas

Este artigo detalhou como usar `kured` reinicializar nós do Linux automaticamente como parte do processo de atualização de segurança. Para fazer upgrade para a última versão do Kubernetes, você pode [fazer upgrade do seu cluster do AKS][aks-upgrade].

Para clusters AKS que usam nós do Windows Server, consulte [Upgrade de um pool de nós no AKS][nodepool-upgrade].

<!-- LINKS - external -->
[kured]: https://github.com/weaveworks/kured
[kured-install]: https://github.com/weaveworks/kured#installation
[kubectl-get-nodes]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[DaemonSet]: concepts-clusters-workloads.md#statefulsets-and-daemonsets
[aks-ssh]: ssh.md
[aks-upgrade]: upgrade-cluster.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
