---
title: Implantar a solução de monitoramento remoto localmente (via IntelliJ IDE) – Azure | Microsoft Docs
description: Este guia de instruções mostra como implantar o acelerador de solução de monitoramento remoto no computador local usando o IntelliJ para teste e desenvolvimento.
author: v-krghan
manager: dominicbetts
ms.author: v-krghan
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/24/2019
ms.topic: conceptual
ms.openlocfilehash: 2b55fea69fe1affb6cab5d360f1e8355c3bb720d
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "66015444"
---
# <a name="deploy-the-remote-monitoring-solution-accelerator-locally---intellij"></a>Implantar o acelerador de solução de Monitoramento Remoto localmente – IntelliJ

[!INCLUDE [iot-accelerators-selector-local](../../includes/iot-accelerators-selector-local.md)]

Este artigo mostra como implantar o acelerador de solução de Monitoramento Remoto no computador local para teste e desenvolvimento. Você aprenderá como executar os microsserviços no IntelliJ. Uma implantação de microsserviços local usa os seguintes serviços de nuvem: Hub IoT, Cosmos DB, Azure Stream Analytics e serviços na nuvem do Azure Time Series Insights.

Se você quiser executar o acelerador de solução de Monitoramento Remoto no Docker em seu computador local, confira [Implantar o acelerador de solução de Monitoramento Remoto localmente – Docker](iot-accelerators-remote-monitoring-deploy-local-docker.md).

## <a name="prerequisites"></a>Pré-requisitos

Para implantar os serviços do Azure usados pelo acelerador de solução de Monitoramento Remoto, você precisará de uma assinatura ativa do Azure.

Se você não tiver uma conta, poderá criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [Avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).

### <a name="machine-setup"></a>Configuração do computador

Para concluir a implantação local, você precisa ter as seguintes ferramentas instaladas no computador de desenvolvimento local:

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/)
* [IntelliJ Plugin Scala](https://plugins.jetbrains.com/plugin/1347-scala)
* [IntelliJ Plugin SBT](https://plugins.jetbrains.com/plugin/5007-sbt)
* [IntelliJ Plugin SBT Executor](https://plugins.jetbrains.com/plugin/7247-sbt-executor)
* [Nginx](https://nginx.org/en/download.html)
* [Node.js v8](https://nodejs.org/) – este software é um pré-requisito para a CLI do PCS que os scripts usam para criar recursos do Azure. Não use o Node.js v10.

> [!NOTE]
> O IntelliJ IDE está disponível para Windows e Mac.

## <a name="download-the-source-code"></a>Fazer o download do código-fonte

Os repositórios de código-fonte de Monitoramento Remoto incluem o código-fonte e os arquivos de configuração do Docker necessários para executar as imagens do Docker dos microsserviços.

Para clonar e criar uma versão local do repositório, use o ambiente da linha de comando para navegar até uma pasta adequada no computador local. Em seguida, execute um dos seguintes conjuntos de comandos para clonar o repositório Java:

Para baixar a versão mais recente das implementações do microsserviço Java, execute:


```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> Esses comandos baixam o código-fonte para todos os microsserviços, bem como os scripts usados para executar os microsserviços localmente. Embora o código-fonte não seja necessário para executar os microsserviços no Docker, ele é útil se você planeja modificar o acelerador de solução e testar suas alterações localmente no futuro.

## <a name="deploy-the-azure-services"></a>Implantar os serviços do Azure

Embora este artigo mostre como executar os microsserviços localmente, eles dependem de serviços do Azure em execução na nuvem. Use o seguinte script a seguir para implantar os serviços do Azure. Os exemplos de script a seguir pressupõem que você esteja usando o repositório Java em um computador Windows. Se estiver trabalhando em outro ambiente, ajuste os caminhos, extensões de arquivo e separadores de caminho adequadamente.

### <a name="create-new-azure-resources"></a>Criar novos recursos do Azure

Se você ainda não criou os recursos do Azure necessários, siga estas etapas:

1. No ambiente da linha de comando, navegue até a pasta **\services\scripts\local\launch** em sua cópia clonada do repositório.

1. Execute os seguintes comandos para instalar as ferramentas da CLI **pcs** e entrar em sua conta do Azure:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Execute o script **start.cmd**. O script solicitará as seguintes informações:
   * Um nome de solução.
   * A assinatura do Azure a utilizar.
   * O local do datacenter do Azure para usar.

     O script cria um grupo de recursos no Azure com o nome da solução. Esse grupo de recursos contém os recursos do Azure usados pelo acelerador de solução. Depois que você não precisar mais dos recursos correspondentes, você pode excluir esse grupo de recursos.

     O script também adiciona um conjunto de variáveis de ambiente com um prefixo **PCS** em seu computador local. Essas variáveis de ambiente fornecem os detalhes de monitoramento remoto ser capaz de ler de um recurso do Azure Key Vault. Esse recurso do Key Vault é onde o monitoramento remoto será leitura seus valores de configuração do.

     > [!TIP]
     > Quando o script é concluído, ele também salva as variáveis de ambiente para um arquivo chamado **\<sua pasta base\>\\.pcs\\\<nome da solução\>.env**. Você pode usá-las para implantações futuras de acelerador de solução. Observe que quaisquer variáveis de ambiente definidas em seu computador local substituem os valores no arquivo **serviços\\scripts\\local\\.env** ao executar **docker-compose**.

1. Saia do ambiente de linha de comando.

### <a name="use-existing-azure-resources"></a>Usar recursos existentes do Azure

Se você já tiver criado os recursos necessários do Azure, crie as variáveis de ambiente correspondentes no seu computador local.
Defina as variáveis de ambiente para o seguinte:
* **PCS_KEYVAULT_NAME** -nome do recurso do Azure Key Vault
* **PCS_AAD_APPID** -ID do aplicativo do AAD
* **PCS_AAD_APPSECRET** -segredo do aplicativo do AAD

Valores de configuração serão lidos desse recurso de Cofre de chaves do Azure. Essas variáveis de ambiente podem ser salvos na  **\<sua pasta base\>\\PCs\\\<nome da solução\>env** arquivo da implantação. Observe que quaisquer variáveis de ambiente definidas em seu computador local substituem os valores no arquivo **serviços\\scripts\\local\\.env** ao executar **docker-compose**.

Algumas das configurações necessárias para o microsserviço é armazenado em uma instância do **Key Vault** que foi criado na implantação inicial. As variáveis correspondentes no cofre de chaves devem ser modificadas conforme necessário.

## <a name="run-the-microservices"></a>Executar os microsserviços

Nesta seção, você executa os microsserviços do Monitoramento Remoto. Execute a interface de usuário Web nativamente, o serviço de Simulação de Dispositivo, o serviço de Autenticação e Gerenciador do ASA no Docker e os microsserviços no IntelliJ.

### <a name="run-the-device-simulation-service"></a>Executar o serviço de simulação de dispositivo

Abra uma nova janela do prompt de comando para ter certeza de que você tem acesso às variáveis de ambiente definidas pelo script **start.cmd** na seção anterior.

Execute o seguinte comando para iniciar o contêiner do Docker para o serviço de simulação de dispositivo. O serviço simula os dispositivos para a solução de monitoramento remoto.

```cmd
<path_to_cloned_repository>\services\device-simulation\scripts\docker\run.cmd
```

### <a name="run-the-auth-service"></a>Executar o serviço de Autenticação

Abra uma nova janela do prompt de comando e execute o comando a seguir para iniciar o contêiner do Docker para o serviço de Autenticação. O serviço permite gerenciar os usuários autorizados a acessar soluções de IoT do Azure.

```cmd
<path_to_cloned_repository>\services\auth\scripts\docker\run.cmd
```

### <a name="run-the-asa-manager-service"></a>Executar o serviço do Gerenciador do ASA

Abra uma nova janela do prompt de comando e execute o comando a seguir para iniciar o contêiner do Docker para o serviço do Gerenciador do ASA. O serviço permite o gerenciamento de trabalhos do Azure Stream Analytics (ASA), incluindo configurar e iniciar, interromper e monitorar status.

```cmd
<path_to_cloned_repository>\services\asa-manager\scripts\docker\run.cmd
```

### <a name="deploy-all-other-microservices-on-local-machine"></a>Implantar todos os outros microsserviços na máquina local

As etapas a seguir mostram como executar os microsserviços de Monitoramento Remoto no IntelliJ:

#### <a name="import-project"></a>Importar projeto

1. Abra o IntelliJ IDE
1. Selecione **Importar projeto** e escolha **azure-iot-pcs-remote-monitoring-java\services\build.sbt**

#### <a name="create-run-configurations"></a>Criar configurações de execução

1. Selecione **Executar > Editar configurações**
1. Selecione **Adicionar uma nova configuração > tarefa sbt** 
1. Insira o **Nome** e defina **Tarefas**, como executar 
1. Selecione o **Diretório de Trabalho** com base no serviço que você deseja executar
1. Clique em **Aplicar > Ok** para salvar suas escolhas.
1. Crie configurações de execução para os seguintes serviços:
    * WebService (services\config)
    * WebService (services\device-telemetry)
    * WebService (services\iothub-manager)
    * WebService (services\storage-adapter)

Por exemplo, a imagem a seguir mostra a adição de configuração para um serviço:

[![Add-Configuration](./media/deploy-locally-intellij/run-configurations.png)](./media/deploy-locally-intellij/run-configurations.png#lightbox)


#### <a name="create-compound-configuration"></a>Criar configuração composta

1. Para executar todos os serviços, selecione todos e escolha **Adicionar uma nova configuração > Composta**
1. Insira o **Nome** e **adicione tarefas sbt**
1. Clique em **Aplicar > Ok** para salvar suas escolhas.

Por exemplo, a imagem a seguir mostra a adição de todas as tarefas de sbt a única configuração:

[![Add-All-Services](./media/deploy-locally-intellij/all-services.png)](./media/deploy-locally-intellij/all-services.png#lightbox)

Clique em **Executar** para compilar e executar os serviços Web no computador local.

Cada serviço Web abre uma janela do navegador e do prompt de comando. No prompt de comando, há uma saída do serviço em execução, e a janela do navegador permite que você monitore o status. Não feche os prompts de comando ou páginas da Web, essa ação interrompe o serviço Web.


Para acessar o status dos serviços, navegue até as seguintes URLs:
* Gerenciador do Hub IoT [http://localhost:9002/v1/status](http://localhost:9002/v1/status)
* Telemetria de Dispositivo [http://localhost:9004/v1/status](http://localhost:9004/v1/status)
* config [http://localhost:9005/v1/status](http://localhost:9005/v1/status)
* storage-adapter [http://localhost:9022/v1/status](http://localhost:9022/v1/status)


### <a name="start-the-stream-analytics-job"></a>Iniciar o trabalho do Stream Analytics

Execute estas etapas para iniciar o trabalho do Stream Analytics:

1. Navegue até o [Portal do Azure](https://portal.azure.com).
1. Navegue até o **Grupo de recursos** criado para sua solução. O nome do grupo de recursos é o nome da solução que você escolheu quando executou o script **start.cmd**.
1. Clique no **trabalho do Stream Analytics** na lista de recursos.
1. Na página **visão geral** do trabalho do Stream Analytics, clique no botão **Iniciar**. Clique em **Iniciar** para iniciar o trabalho.

### <a name="run-the-web-ui"></a>Executar a interface do usuário da Web

Nesta etapa, você inicia a interface do usuário da Web. Abra uma nova janela do prompt de comando para ter certeza de que você tem acesso às variáveis de ambiente definidas pelo script **start.cmd**. Navegue até a pasta **webui** em sua cópia local do repositório e execute os seguintes comandos:

```cmd
npm install
npm start
```

Quando o início for concluído, o navegador exibe a página **http:\//localhost:3000 / dashboard**. São esperados erros nessa página. Para exibir o aplicativo sem erros, conclua a etapa a seguir.

### <a name="configure-and-run-nginx"></a>Configurar e executar o NGINX

Configure um servidor proxy reverso para vincular o aplicativo Web e os microsserviços em execução no computador local:

* Copie o arquivo **nginx.conf** da pasta **webui\scripts\localhost** em sua cópia local do repositório para o diretório de instalação **nginx\conf**.
* Execute **nginx**.

Para saber mais sobre como executar **nginx**, confira [nginx para Windows](https://nginx.org/en/docs/windows.html).

### <a name="connect-to-the-dashboard"></a>Conectar-se ao painel

Para acessar o painel de solução de monitoramento remoto, navegue para http:\//localhost:9000 no seu navegador.

## <a name="clean-up"></a>Limpar

Para evitar encargos desnecessários, quando terminar o teste, remova os serviços de nuvem de sua assinatura do Azure. Para remover os serviços, navegue até o [portal do Azure](https://ms.portal.azure.com) e exclua o grupo de recursos que o script **start.cmd** criou.

Você também pode excluir a cópia local do repositório de Monitoramento Remoto criada quando você clonou o código-fonte do GitHub.

## <a name="next-steps"></a>Próximas etapas

Agora que você implantou a solução de monitoramento remoto, a próxima etapa é [explorar os recursos do painel de solução](quickstart-remote-monitoring-deploy.md).
