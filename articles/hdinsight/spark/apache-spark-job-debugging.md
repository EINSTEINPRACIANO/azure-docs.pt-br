---
title: Depurar trabalhos do Apache Spark em execução no Azure HDInsight
description: Use a interface do usuário do YARN, a interface do usuário do Spark e o Servidor de Histórico do Spark para rastrear e depurar trabalhos em execução no cluster Spark no Azure HDInsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: hrasheed
ms.openlocfilehash: 5e384520c1b8d6cf5e3b182bbddf41a5f4f7f8f6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707816"
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Depurar trabalhos do Apache Spark em execução no Azure HDInsight

Neste artigo, você aprende como rastrear e depurar as tarefas do [Apache Spark](https://spark.apache.org/) em execução nos clusters do HDInsight usando a interface do usuário do [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), a IU do Spark e o Servidor do histórico do Spark. Você começará um trabalho do Spark usando um notebook disponível com o cluster Spark, **Machine Learning: Análise preditiva em dados de inspeção de alimentos usando MLLib**. Você pode usar as etapas a seguir para rastrear um aplicativo que foi enviado usando qualquer outra abordagem, por exemplo, **spark-submit**.

## <a name="prerequisites"></a>Pré-requisitos
Você deve ter o seguinte:

* Uma assinatura do Azure. Consulte [Obter a avaliação gratuita do Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Um cluster do Apache Spark no HDInsight. Para obter instruções, consulte o artigo sobre como [Criar clusters do Apache Spark no Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* Você deve ter começado a executar o notebook **[Machine Learning: Análise preditiva em dados de inspeção de alimentos usando MLLib](apache-spark-machine-learning-mllib-ipython.md)**. Para obter instruções sobre como executar este notebook, siga o link.  

## <a name="track-an-application-in-the-yarn-ui"></a>Rastrear um aplicativo na interface do usuário do YARN
1. Inicie a interface do usuário do YARN. Clique em **Yarn** em **painéis do Cluster**.
   
    ![Iniciar Interface do usuário do YARN](./media/apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]  
   > Alternativamente, também é possível iniciar a interface do usuário do YARN na interface do usuário do Ambari. Para iniciar a UI do Ambari, clique **Ambari doméstica** sob **painéis do Cluster**. Na interface do usuário do Ambari, clique em **YARN**, em **Links Rápidos**, no Resource Manager ativo e, por fim, em **Interface do Usuário do Resource Manager**. 

2. Como você iniciou o trabalho do Spark usando notebooks Jupyter, o aplicativo tem o nome **remotesparkmagics** (esse é o nome de todos os aplicativos que são iniciados do notebook). Clique na ID de aplicativo com o nome do aplicativo para obter mais informações sobre o trabalho. Isso inicia o modo de exibição do aplicativo.
   
    ![Localizar a ID de aplicativo Spark](./media/apache-spark-job-debugging/find-application-id.png)
   
    Para aplicativos que são iniciados do notebook Jupyter, o status é sempre **EM EXECUÇÃO** até que você saia do notebook.
3. Na exibição de aplicativo, você pode fazer drill down para descobrir os contêineres associados ao aplicativo e os logs (stdout/stderr). Você também pode iniciar a interface do usuário do Spark clicando no link correspondente para a **URL de Rastreamento**, conforme mostrado abaixo. 
   
    ![Baixar logs de contêiner](./media/apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-the-spark-ui"></a>Rastrear um aplicativo na interface do usuário do Spark
Na interface do usuário do Spark, é possível fazer drill down em trabalhos do Spark que são gerados pelo aplicativo iniciado anteriormente.

1. Para iniciar a interface do usuário do Spark, da exibição do aplicativo, clique no link em **URL de Rastreamento**, conforme mostrado na captura de tela acima. Você pode ver todos os trabalhos do Spark que são iniciados pelo aplicativo em execução no notebook do Jupyter.
   
    ![Exibir trabalhos do Spark](./media/apache-spark-job-debugging/view-spark-jobs.png)
2. Clique na guia **Executores** para ver informações de processamento e armazenamento de cada executor. Você também pode recuperar a pilha de chamadas clicando no link **Thread Dump** (Despejo de Thread).
   
    ![Exibir executores do Spark](./media/apache-spark-job-debugging/view-spark-executors.png)
3. Clique na guia **Estágios** para ver os estágios associados ao aplicativo.
   
    ![Exibir estágios do Spark](./media/apache-spark-job-debugging/view-spark-stages.png)
   
    Cada estágio pode ter várias tarefas para as quais você pode exibir estatísticas de execução, como mostrado abaixo.
   
    ![Exibir estágios do Spark](./media/apache-spark-job-debugging/view-spark-stages-details.png) 
4. Na página de detalhes do estágio, você pode iniciar Visualização de DAG. Expanda o link **DAG Visualization** (Visualização de DAG) na parte superior da página, como mostrado abaixo.
   
    ![Exibir a visualização de DAG dos estágios do Spark](./media/apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    O DAG ou Grafo Acíclico Direto representa os diferentes estágios no aplicativo. Cada caixa azul no grafo representa uma operação do Spark iniciada do aplicativo.
5. Na página de detalhes do estágio, você também pode iniciar o modo de exibição de linha do tempo do aplicativo. Expanda o link **Event Timeline** (Linha do Tempo do Evento) na parte superior da página, como mostrado abaixo.
   
    ![Exibir linha do tempo de evento de estágios do Spark](./media/apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Isso exibe os eventos do Spark na forma de uma linha do tempo. O modo de exibição de tempo está disponível em três níveis, entre trabalhos, dentro de um trabalho e dentro de um estágio. A imagem acima captura o modo de exibição de linha do tempo de um determinado estágio.
   
   > [!TIP]  
   > Se você selecionar a caixa de seleção **Enable zooming** (Habilitar zoom), poderá rolar para a esquerda e para a direita no modo de exibição de linha do tempo.

6. Outras guias na interface do usuário do Spark fornecem informações úteis sobre a instância do Spark.
   
   * Guia Armazenamento: se seu aplicativo criar RDDs, você encontrará informações sobre isso na guia Armazenamento.
   * Guia Ambiente: esta guia fornece muitas informações úteis sobre sua instância do Spark, como a 
     * Versão da escala
     * Diretório de log de eventos associado ao cluster
     * Número de núcleos de executor do aplicativo
     * Etc.

## <a name="find-information-about-completed-jobs-using-the-spark-history-server"></a>Encontrar informações sobre trabalhos concluídos usando o Servidor de Histórico do Spark
Quando um trabalho é concluído, as informações sobre ele são mantidas no Servidor de Histórico do Spark.

1. Para iniciar o Servidor de Histórico do Spark, na folha de Visão geral, clique em **Servidor de histórico Spark** em **Painéis do cluster**.
   
    ![Iniciar o Servidor de Histórico do Spark](./media/apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]  
   > Alternativamente, também é possível iniciar a interface do usuário do Servidor de Histórico do Spark na interface do usuário do Ambari. Para iniciar a UI do Ambari, na folha de visão geral, clique em **Ambari home** em **Painéis de Cluster**. Na interface do usuário do Ambari, clique em **Spark**, clique em **Links Rápidos**, e, em seguida, clique em **Spark History Server UI** (Interface do usuário do Servidor de Histórico do Spark).

2. Você verá todos os aplicativos concluídos listados. Clique em uma ID de aplicativo para fazer o drill down em um aplicativo para obter mais informações.
   
    ![Iniciar o Servidor de Histórico do Spark](./media/apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a>Consulte também
*  [Gerenciar os recursos de cluster do Apache Spark no Azure HDInsight](apache-spark-resource-manager.md)
*  [Depure as tarefas do Spark do Apache usando o Extended History Server estendido](apache-azure-spark-history-server.md)

### <a name="for-data-analysts"></a>Para analistas de dados

* [Apache Spark com Machine Learning: Usar o Spark no HDInsight para analisar a temperatura de prédios usando dados do sistema de HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark com Machine Learning: Usar o Spark no HDInsight para prever resultados da inspeção de alimentos](apache-spark-machine-learning-mllib-ipython.md)
* [Análise de log do site usando o Apache Spark no HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Análise de dados do Application Insight telemetria usando o Apache Spark no HDInsight](apache-spark-analyze-application-insight-logs.md)
* [Usar o Caffe no Azure HDInsight Spark para aprendizado aprofundado distribuído](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Para desenvolvedores do Spark

* [Criar um aplicativo autônomo usando Scala](apache-spark-create-standalone-application.md)
* [Execute trabalhos remotamente em um cluster do Apache Spark usando o Apache Livy](apache-spark-livy-rest-interface.md)
* [Use o Plug-in de Ferramentas do HDInsight para IntelliJ IDEA para criar e enviar aplicativos Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Use o Plugin do HDInsight Tools para o IntelliJ IDEA para depurar os aplicativos do Apache Spark remotamente](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Use os blocos de anotações do Apache Zeppelin com um cluster do Apache Spark no HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponíveis para o notebook Jupyter no cluster do Apache Spark para HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Usar pacotes externos com blocos de notas Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter em seu computador e conectar-se a um cluster Spark do HDInsight](apache-spark-jupyter-notebook-install-locally.md)
