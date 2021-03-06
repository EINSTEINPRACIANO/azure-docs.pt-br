---
title: Criar um aplicativo Web do Java no Linux – Serviço de Aplicativo do Azure
description: Neste início rápido, você implanta seu primeiro Olá, Mundo em Java no Serviço de Aplicativo do Azure no Linux em minutos.
keywords: azure, serviço de aplicativo, aplicativo Web, linux, java, maven, início rápido
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/27/2019
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: 7c6d5034335a455d24b1f22919b672e2ead2810d
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65510853"
---
# <a name="quickstart-create-a-java-app-in-app-service-on-linux"></a>Início Rápido: Criar um aplicativo Java no Serviço de Aplicativo no Linux

O [Serviço de Aplicativo no Linux](app-service-linux-intro.md) fornece um serviço de hospedagem na Web altamente escalonável e com aplicação automática de patches usando o sistema operacional Linux. Este início rápido mostra como usar a [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) com o [Plug-in do Maven para o Serviço de Aplicativo do Azure](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) para implantar um arquivo WAR (Arquivo Web) Java.
> [!NOTE]
>
> A mesma coisa também pode ser feita usando IDEs populares, como o IntelliJ e Eclipse. Confira nossos documentos semelhantes em [Início Rápido: Azure Toolkit for IntelliJ](/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app) ou [Início Rápido: Azure Toolkit for Eclipse](/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app).
>
![Aplicativo de exemplo em execução no Azure](media/quickstart-java/java-hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Criar um aplicativo Java

Execute o seguinte comando do Maven no prompt do Cloud Shell para criar um aplicativo chamado `helloworld`:

```bash
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp
```

## <a name="configure-the-maven-plugin"></a>Configurar o plug-in do Maven

Para implantar do Maven, use o editor de códigos no Cloud Shell para abrir arquivo de projeto `pom.xml` no diretório `helloworld`. 

```bash
code pom.xml
```

Em seguida, adicione a seguinte definição de plug-in ao elemento `<build>` do arquivo `pom.xml`.

```xml
<plugins>
    <!--*************************************************-->
    <!-- Deploy to Tomcat in App Service Linux           -->
    <!--*************************************************-->
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.5.4</version>
        <configuration>
            <!-- Specify v2 schema -->
            <schemaVersion>v2</schemaVersion>
            <!-- App information -->
            <subscriptionId>${SUBSCRIPTION_ID}</subscriptionId>
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
   
            <!-- Java Runtime Stack for App on Linux-->
            <runtime>
                <os>linux</os>
                <javaVersion>jre8</javaVersion>
                <webContainer>tomcat 8.5</webContainer>
            </runtime> 
            <deployment>
                <resources>
                    <resource>
                        <directory>${project.basedir}/target</directory>
                        <includes>
                            <include>*.war</include>
                        </includes>
                    </resource>
                </resources>
            </deployment>
        </configuration>
    </plugin>
</plugins>
```    


> [!NOTE] 
> Neste artigo, estamos trabalhando apenas com aplicativos Java empacotados em arquivos WAR. O plug-in também oferece suporte a aplicativos Web JAR, visite [Implantar um arquivo JAR do Java SE para o Serviço de Aplicativo no Linux](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) para experimentá-lo.


Atualize os seguintes espaços reservados na configuração do plug-in:

| Placeholder | DESCRIÇÃO |
| ----------- | ----------- |
| `SUBSCRIPTION_ID` | A ID exclusiva da assinatura em que você deseja implantar seu aplicativo. A ID da assinatura padrão pode ser encontrada no Cloud Shell ou na CLI usando o comando `az account show`. Para todas as assinaturas disponíveis, use o comando `az account list`.|
| `RESOURCEGROUP_NAME` | Nome do novo grupo de recursos no qual criar o aplicativo. Ao colocar todos os recursos para um aplicativo em um grupo, você pode gerenciá-los juntos. Por exemplo, excluir o grupo de recursos excluiria todos os recursos associados ao aplicativo. Atualize esse valor com um novo nome de grupo de recursos exclusivo, por exemplo, *TestResources*. Você usará esse nome de grupo de recursos para limpar todos os recursos do Azure em uma seção posterior. |
| `WEBAPP_NAME` | O nome do aplicativo fará parte do nome do host do aplicativo quando implantado no Azure (NOME_APLICATIVO_WEB.azurewebsites.net). Atualize esse valor com um nome exclusivo para o novo aplicativo do Serviço de Aplicativo, que hospedará o aplicativo Java, por exemplo, *contoso*. |
| `REGION` | Uma região do Azure na qual o aplicativo está hospedado, por exemplo, `westus2`. Você pode obter uma lista de regiões do Cloud Shell ou da CLI usando o comando `az account list-locations`. |

## <a name="deploy-the-app"></a>Implantar o aplicativo

Implante seu aplicativo Java no Azure usando o seguinte comando:

```bash
mvn package azure-webapp:deploy
```

Após a conclusão da implantação, navegue até o aplicativo implantado usando a URL a seguir no navegador da Web, por exemplo, `http://<webapp>.azurewebsites.net`. 

![Aplicativo de exemplo em execução no Azure](media/quickstart-java/java-hello-world-in-browser-curl.png)

**Parabéns!** Você implantou seu primeiro aplicativo Java no Serviço de Aplicativo no Linux.

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Tutorial: Aplicativo Java Enterprise com o PostgreSQL](tutorial-java-enterprise-postgresql-app.md)

> [!div class="nextstepaction"]
> [Configurar o aplicativo Java](configure-custom-container.md)

> [!div class="nextstepaction"]
> [CI/CD com Jenkins](/azure/jenkins/deploy-jenkins-app-service-plugin)

> [!div class="nextstepaction"]
> [Outros recursos do Azure para desenvolvedores Java](/java/azure/)
