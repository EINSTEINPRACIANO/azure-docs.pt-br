---
title: Definir variáveis de ambiente em Instâncias de Contêiner do Azure
description: Saiba como definir variáveis de ambiente nos contêineres que você executa nas Instâncias de Contêiner do Azure
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/17/2019
ms.author: danlep
ms.openlocfilehash: 4a4b19338d96094f28b4f4bedd8042723f67f10a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66149136"
---
# <a name="set-environment-variables-in-container-instances"></a>Definir variáveis de ambiente em instâncias de contêiner

A definição de variáveis de ambiente nas suas instâncias de contêiner permitem fornecer a configuração dinâmica do aplicativo ou script executado pelo contêiner. Isso é semelhante ao argumento de linha de comando `--env` para `docker run`. 

Para definir variáveis de ambiente em um contêiner, especifique-as ao criar uma instância de contêiner. Este artigo mostra exemplos de como definir variáveis de ambiente quando você inicia um contêiner com o [CLI do Azure](#azure-cli-example), [do Azure PowerShell](#azure-powershell-example)e o [portal do Azure](#azure-portal-example). 

Por exemplo, se você executar o Microsoft [aci-wordcount] [ aci-wordcount] imagem de contêiner, você pode modificar seu comportamento especificando as seguintes variáveis de ambiente:

*NumWords*: o número de palavras enviadas para STDOUT.

*MinLength*: o número mínimo de caracteres em uma palavra para que ela seja contada. Um número mais alto ignora palavras comuns como "de" e "a" ou “o”.

Se for necessário passar segredos como variáveis de ambiente, as Instâncias de Contêiner do Azure dão suporte a [valores seguros](#secure-values) para contêineres do Windows e do Linux.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="azure-cli-example"></a>Exemplos de CLI do Azure

Para ver a saída padrão do [aci-wordcount] [ aci-wordcount] contêiner, executá-lo pela primeira vez com essa [criar contêiner az] [ az-container-create] (não de comando variáveis de ambiente especificadas):

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer1 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure
```

Para modificar a saída, inicie um segundo contêiner com o argumento `--environment-variables` incluído, especificando valores para as variáveis *NumWords* e *MinLength*. (Este exemplo pressupõe que você esteja executando a CLI em um shell Bash ou no Azure Cloud Shell. Se você usar o Prompt de Comando do Windows, especifique as variáveis com aspas duplas, como `--environment-variables "NumWords"="5" "MinLength"="8"`.)

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainer2 \
    --image mcr.microsoft.com/azuredocs/aci-wordcount:latest \
    --restart-policy OnFailure \
    --environment-variables 'NumWords'='5' 'MinLength'='8'
```

Quando o estado de ambos os contêineres for exibido como *Encerrado* (use [az container show][az-container-show] para verificar o estado), exiba os registros com [az container logs][az-container-logs] para ver a saída.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer1
az container logs --resource-group myResourceGroup --name mycontainer2
```

A saída dos contêineres mostra como você modificou o comportamento do script do segundo container definindo variáveis de ambiente.

```console
azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

azureuser@Azure:~$ az container logs --resource-group myResourceGroup --name mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]
```

## <a name="azure-powershell-example"></a>Exemplo de Azure PowerShell

Definir variáveis de ambiente no PowerShell é semelhante à CLI, mas usa o argumento de linha de comando `-EnvironmentVariable`.

Primeiro, inicie o [aci-wordcount] [ aci-wordcount] contêiner em sua configuração padrão com este [New-AzContainerGroup] [ new-Azcontainergroup] comando:

```azurepowershell-interactive
New-AzContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer1 `
    -Image mcr.microsoft.com/azuredocs/aci-wordcount:latest
```

Agora execute o seguinte [New-AzContainerGroup] [ new-Azcontainergroup] comando. Este especifica as variáveis de ambiente *NumWords* e *MinLength* depois de preencher uma variável de matriz `envVars`:

```azurepowershell-interactive
$envVars = @{'NumWords'='5';'MinLength'='8'}
New-AzContainerGroup `
    -ResourceGroupName myResourceGroup `
    -Name mycontainer2 `
    -Image mcr.microsoft.com/azuredocs/aci-wordcount:latest `
    -RestartPolicy OnFailure `
    -EnvironmentVariable $envVars
```

Depois que o estado dos dois contêineres está *encerrado* (usar [Get-AzContainerInstanceLog] [ azure-instance-log] para verificar o estado), extrair seus logs com o [ Get-AzContainerInstanceLog] [ azure-instance-log] comando.

```azurepowershell-interactive
Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
```

A saída de cada contêiner mostra como você modificou o script executado pelo contêiner, definindo variáveis de ambiente.

```console
PS Azure:\> Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer1
[('the', 990),
 ('and', 702),
 ('of', 628),
 ('to', 610),
 ('I', 544),
 ('you', 495),
 ('a', 453),
 ('my', 441),
 ('in', 399),
 ('HAMLET', 386)]

Azure:\
PS Azure:\> Get-AzContainerInstanceLog -ResourceGroupName myResourceGroup -ContainerGroupName mycontainer2
[('CLAUDIUS', 120),
 ('POLONIUS', 113),
 ('GERTRUDE', 82),
 ('ROSENCRANTZ', 69),
 ('GUILDENSTERN', 54)]

Azure:\
```

## <a name="azure-portal-example"></a>Exemplo do portal do Azure

Para definir variáveis de ambiente quando você inicia um contêiner no portal do Azure, especifique-os na **avançado** página quando você criar o contêiner.

1. Sobre o **avançado** , defina as **política de reinicialização** para *em caso de falha*
2. Sob **variáveis de ambiente**, insira `NumWords` com um valor de `5` para a primeira variável e insira `MinLength` com um valor de `8` para a segunda variável. 
1. Selecione **revisar + criar** para verificar e, em seguida, implantar o contêiner.

![Página do portal mostrando a variável de ambiente Habilitar botão e caixas de texto][portal-env-vars-01]

Para exibir os logs do contêiner, em **as configurações** selecionar **contêineres**, em seguida, **Logs**. Semelhante à saída mostrada nas seções CLI e PowerShell anteriores, é possível ver como o comportamento do script foi modificado pelas variáveis de ambiente. Apenas cinco palavras são exibidas, cada uma com um comprimento mínimo de oito caracteres.

![Portal mostrando a saída do log de contêiner][portal-env-vars-02]

## <a name="secure-values"></a>Valores seguros

Objetos com valores seguros servem para proteger informações confidenciais como senhas ou chaves para seu aplicativo. Usar valores seguros para variáveis de ambiente é mais seguro e flexível do que incluí-los na imagem de contêiner. Outra opção é usar os volumes secretos, descritos em [Montar um volume secreto em Instâncias de Contêiner do Azure](container-instances-volume-secret.md).

Variáveis de ambiente com valores seguros não são visíveis nas propriedades do contêiner – os valores podem ser acessados somente por dentro do contêiner. Por exemplo, as propriedades de contêiner exibidas no portal do Azure ou na CLI do Azure exibirão apenas o nome de uma variável segura, mas não o valor.

A variável de ambiente seguro podem ser definida especificando a propriedade `secureValue` em vez de `value` para o tipo da variável. As duas variáveis definidas no YAML a seguir demonstram os dois tipos de variável.

### <a name="yaml-deployment"></a>Implantação do YAML

Crie um arquivo `secure-env.yaml` com o snippet a seguir.

```yaml
apiVersion: 2018-10-01
location: eastus
name: securetest
properties:
  containers:
  - name: mycontainer
    properties:
      environmentVariables:
        - name: 'NOTSECRET'
          value: 'my-exposed-value'
        - name: 'SECRET'
          secureValue: 'my-secret-value'
      image: nginx
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Execute o seguinte comando para implantar o grupo de contêineres com YAML (ajuste o nome do grupo de recursos conforme necessário):

```azurecli-interactive
az container create --resource-group myResourceGroup --file secure-env.yaml
```

### <a name="verify-environment-variables"></a>Verifique as variáveis de ambiente

Execute o comando [az container show][az-container-show] para consultar as variáveis de ambiente do contêiner:

```azurecli-interactive
az container show --resource-group myResourceGroup --name securetest --query 'containers[].environmentVariables'
```

A resposta JSON mostra a chave e o valor de ambiente não seguro, mas apenas o nome da variável de ambiente seguro:

```json
[
  [
    {
      "name": "NOTSECRET",
      "secureValue": null,
      "value": "my-exposed-value"
    },
    {
      "name": "SECRET",
      "secureValue": null,
      "value": null
    }
  ]
]
```

Com o comando [az container exec][az-container-exec], que permite executar um comando dentro de um contêiner em execução, é possível verificar se a variável de ambiente seguro foi definida. Execute o comando a seguir para iniciar uma sessão interativa de busca no contêiner:

```azurecli-interactive
az container exec --resource-group myResourceGroup --name securetest --exec-command "/bin/bash"
```

Depois de abrir um shell interativo dentro do contêiner, é possível acessar o valor da variável `SECRET`:

```console
root@caas-ef3ee231482549629ac8a40c0d3807fd-3881559887-5374l:/# echo $SECRET
my-secret-value
```

## <a name="next-steps"></a>Próximas etapas

Cenários baseados em tarefas, como o processamento em lote de um grande conjunto de dados com vários contêineres, podem se beneficiar de variáveis de ambiente personalizadas no tempo de execução. Para obter mais informações sobre como executar contêineres baseados em tarefas, consulte [executar tarefas em contêineres com diretivas de reinicialização](container-instances-restart-policy.md).

<!-- IMAGES -->
[portal-env-vars-01]: ./media/container-instances-environment-variables/portal-env-vars-01.png
[portal-env-vars-02]: ./media/container-instances-environment-variables/portal-env-vars-02.png

<!-- LINKS - External -->
[aci-wordcount]: https://hub.docker.com/_/microsoft-azuredocs-aci-wordcount

<!-- LINKS Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[azure-cli-install]: /cli/azure/
[azure-instance-log]: /powershell/module/az.containerinstance/get-azcontainerinstancelog
[azure-powershell-install]: /powershell/azure/install-Az-ps
[new-Azcontainergroup]: /powershell/module/az.containerinstance/new-azcontainergroup
[portal]: https://portal.azure.com
