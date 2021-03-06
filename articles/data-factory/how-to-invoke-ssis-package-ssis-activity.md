---
title: Executar o pacote SSIS com a atividade Executar pacote SSIS – Azure | Microsoft Docs
description: Este artigo descreve como executar um pacote SSIS (SQL Server Integration Services) em um pipeline do Azure Data Factory usando a atividade Executar pacote SSIS.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 03/19/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 7287dc2fccf461cf23c45202336e3d92bc5a40aa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66153206"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>Executar um pacote SSIS com a atividade Executar pacote SSIS no Azure Data Factory
Este artigo descreve como executar um pacote SSIS em um pipeline do Azure Data Factory (ADF) usando a atividade Executar pacote SSIS. 

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Crie um Azure-SSIS Integration Runtime (IR), caso não tenha um, seguindo as instruções passo a passo em [Tutorial: Implantar pacotes SSIS no Azure](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="run-a-package-in-the-azure-portal"></a>Executar um pacote no portal do Azure
Nesta seção, você usa a interface do usuário/aplicativo do ADF (IU) para criar um pipeline do ADF com a atividade Executar pacote SSIS que efetiva o pacote SSIS.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Criar um pipeline com uma atividade Executar Pacote do SSIS
Nesta etapa, você usa a interface do usuário/aplicativo do ADF para criar um pipeline. Adiciona uma atividade Executar pacote SSIS ao pipeline e configure-a para executar o pacote SSIS. 

1. Na visão geral/página inicial do ADF no Portal do Azure, clique no bloco **Autoria e Monitoramento** para iniciar a interface do usuário/aplicativo do ADF em uma guia separada. 

   ![Página inicial do data factory](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)

   Na página **Introdução**, clique em **Criar pipeline**: 

   ![Página Introdução](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)

2. Na caixa de ferramentas **Atividades**, expanda **Geral** e arraste e solte uma atividade **Executar pacote SSIS** para a superfície do designer de pipeline. 

   ![Arraste uma atividade Executar pacote SSIS para a superfície do designer](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

3. Na guia **Geral** para a atividade Executar pacote SSIS, forneça um nome e uma descrição para a atividade. Defina o tempo limite e os valores de repetição opcionais.

   ![Definir propriedades na guia Geral](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

4. Na guia **Configurações** para a atividade Executar pacote SSIS, selecione o Azure-SSIS IR associado ao banco de dados SSISDB em que o pacote está implantado. Se seu pacote usa a autenticação do Windows para acessar armazenamentos de dados, por exemplo, compartilhamentos de arquivo/servidores do SQL no local, arquivos do Azure, etc., verifique as **autenticação do Windows** caixa de seleção e insira o domínio/nome de usuário/senha do pacote execução. Se o seu pacote precisa de tempo de execução de 32 bits para ser executado, marque a caixa de seleção **Tempo de execução de 32 bits**. Em **Nível de registro**, selecione um escopo predefinido de registro em log para a execução do pacote. Marque a caixa de seleção **Personalizado**, se quiser inserir um nome de log personalizado. Quando o Azure-SSIS IR estiver em execução e a caixa de seleção **Entradas Manuais** estiver desmarcada, é possível procurar e selecionar suas pastas/projetos/pacotes/ambientes existentes no SSISDB. Clique no botão **Atualizar** para buscar suas pastas/projetos/pacotes/ambientes recém-adicionados do SSISDB para que eles fiquem disponíveis para navegação e seleção. 

   ![Definir propriedades na guia Configurações — Automático](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

   Quando o IR do Azure-SSIS não está em execução ou o **entradas manuais** caixa de seleção estiver marcada, você pode inserir caminhos de ambiente e pacote de SSISDB diretamente nos seguintes formatos: `<folder name>/<project name>/<package name>.dtsx` e `<folder name>/<environment name>`.

   ![Definir propriedades na guia Configurações — Manual](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings2.png)

5. Na guia **Parâmetros SSIS** da atividade Executar pacote SSIS, quando o Azure-SSIS IR estiver em execução e a caixa de seleção **Entradas manuais** na guia **Configurações** estiver desmarcada, os parâmetros SSIS existentes em seu projeto/pacote selecionado do SSISDB serão exibidos para você atribuir valores a eles. Caso contrário, você pode inseri-los um a um para atribuir valores a eles manualmente. Assegure-se de que eles existam e sejam inseridos corretamente para que a execução do pacote seja bem-sucedida. Você pode adicionar conteúdo dinâmico para seus valores usando expressões, funções, variáveis de sistema do ADF e variáveis/parâmetros de pipeline do ADF. Como alternativa, você pode usar os segredos armazenados em sua chave de cofre AKV (Azure) como seus valores. Para fazer isso, clique no **AZURE KEY VAULT** caixa de seleção ao lado do parâmetro relevante, selecione/Editar seu serviço AKV vinculado existente ou crie um novo e, em seguida, selecione o nome/versão secreto para o valor do parâmetro.  Quando você cria/editar seu serviço AKV vinculado, você pode selecione/Editar seu AKV existente ou crie um novo, mas. conceda acesso de identidade gerenciado do ADF para o AKV se você ainda não fez isso. Você também pode inserir seus segredos diretamente no seguinte formato: `<AKV linked service name>/<secret name>/<secret version>`.

   ![Definir propriedades na guia Parâmetros SSIS](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-ssis-parameters.png)

6. Sobre o **gerenciadores de Conexão** guia para a atividade executar pacote do SSIS, quando o IR Azure-SSIS está em execução e o **entradas manuais** caixa de seleção na **configurações** guia é desmarcado, os gerenciadores de conexão existentes em seu projeto/pacote selecionado do SSISDB serão exibidos para que você possa atribuir valores às suas propriedades. Caso contrário, você pode inseri-los um por um, para atribuir valores às suas propriedades manualmente – Verifique se eles existem e se forem inseridos corretamente para a execução do pacote seja bem-sucedida. Você pode adicionar conteúdo dinâmico para seus valores de propriedade usando expressões, funções, variáveis de sistema do ADF e variáveis/parâmetros de pipeline do ADF. Como alternativa, você pode usar os segredos armazenados em sua chave de cofre AKV (Azure) como seus valores de propriedade. Para fazer isso, clique no **AZURE KEY VAULT** caixa de seleção ao lado da propriedade relevante, selecione/Editar seu serviço AKV vinculado existente ou crie um novo e, em seguida, selecione o nome/versão do segredo para o valor da propriedade.  Quando você cria/editar seu serviço AKV vinculado, você pode selecione/Editar seu AKV existente ou crie um novo, mas. conceda acesso de identidade gerenciado do ADF para o AKV se você ainda não fez isso. Você também pode inserir seus segredos diretamente no seguinte formato: `<AKV linked service name>/<secret name>/<secret version>`.

   ![Definir propriedades na guia Gerenciadores de Conexões](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-connection-managers.png)

7. Na guia **Substituições de Propriedades** para a atividade Executar pacote SSIS, você pode inserir os caminhos das propriedades existentes em seu pacote selecionado do SSISDB, um a um, para atribuir valores a eles manualmente. Assegure-se de que eles existam e sejam inseridos corretamente para que a execução do pacote seja bem-sucedida, por exemplo, para substituir o valor da sua variável de usuário, insira o caminho no seguinte formato: `\Package.Variables[User::YourVariableName].Value`. Você também pode adicionar conteúdo dinâmico aos seus valores usando expressões, funções, variáveis do sistema ADF e parâmetros/variáveis do pipeline do ADF.

   ![Definir propriedades na guia Substituições de Propriedades](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-property-overrides.png)

8. Para validar a configuração do pipeline, clique em **Validar** na barra de ferramentas. Para fechar o **Relatório de validação do pipeline** clique em **>>**.

9. Publique o pipeline no ADF clicando no botão **Publicar tudo**. 

### <a name="run-the-pipeline"></a>Executar o pipeline
Nesta etapa, você dispara uma execução de pipeline. 

1. Para disparar uma execução de pipeline, clique em **Disparar** na barra de ferramentas e clique em **Disparar agora**. 

   ![Disparar agora](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. Na janela **Execução de Pipeline**, selecione **Concluir**. 

### <a name="monitor-the-pipeline"></a>Monitorar o Pipeline

1. Alterne para a guia **Monitorar** à esquerda. Você verá o pipeline de execução e seu status junto com outras informações (como a Hora de início da execução). Para atualizar o modo de exibição, clique em **Atualizar**.

   ![Execuções de pipeline](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

2. Clique no link **Exibir Execuções da atividade** na coluna **Ações**. Você verá apenas uma execução de atividade, pois o pipeline tem apenas uma atividade (Executar Pacote do SSIS).

   ![Execuções de atividade](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

3. É possível executar a seguinte **consulta** no banco de dados SSISDB em seu servidor SQL do Azure para verificar se o pacote foi executado. 

   ```sql
   select * from catalog.executions
   ```

   ![Verificar as execuções do pacote](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

4. Você também pode obter a ID de execução do SSISDB da saída da execução de atividade do pipeline e usá-la para verificar os logs de execução mais abrangentes e mensagens de erro no SSMS.

   ![Obter a ID de execução.](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

### <a name="schedule-the-pipeline-with-a-trigger"></a>Programar o pipeline com um gatilho

Também é possível criar um gatilho agendado para o pipeline, de modo que o pipeline seja executado em um agendamento (por hora, diariamente etc.). Para um exemplo, consulte [Criar uma data factory - Interface do Usuário do Data Factory](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="run-a-package-with-powershell"></a>Executar um pacote com o PowerShell
Nesta seção, você usa o Azure PowerShell para criar um pipeline do ADF com uma atividade Executar pacote SSIS que efetiva o pacote SSIS. 

Instale os módulos mais recentes do Azure PowerShell seguindo as instruções passo a passo em [Como instalar e configurar o Azure PowerShell](/powershell/azure/install-az-ps).

### <a name="create-an-adf-with-azure-ssis-ir"></a>Criar um ADF com o Azure-SSIS IR
Você pode usar um ADF existente que já tenha o Azure-SSIS IR provisionado ou criar um novo ADF com o Azure-SSIS IR seguindo as instruções passo-a-passo em [Tutorial: Implantar pacotes SSIS no Azure via PowerShell](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell).

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Criar um pipeline com uma atividade Executar Pacote do SSIS 
Nesta etapa, você cria um pipeline com uma atividade Executar pacote SSIS. A atividade é executada em seu pacote SSIS. 

1. Crie um arquivo JSON denominado **RunSSISPackagePipeline.json** na pasta **C:\ADF\RunSSISPackage** com o conteúdo semelhante ao deste exemplo:

   > [!IMPORTANT]
   > Substitua os nomes de objeto, as descrições e os caminhos, os valores de propriedade e de parâmetro, as senhas e outros valores variáveis antes de salvar o arquivo. 

   ```json
   {
       "name": "RunSSISPackagePipeline",
       "properties": {
           "activities": [{
               "name": "mySSISActivity",
               "description": "My SSIS package/activity description",
               "type": "ExecuteSSISPackage",
               "typeProperties": {
                   "connectVia": {
                       "referenceName": "myAzureSSISIR",
                       "type": "IntegrationRuntimeReference"
                   },
                   "executionCredential": {
                       "domain": "MyDomain",
                       "userName": "MyUsername",
                       "password": {
                           "type": "SecureString",
                           "value": "**********"
                       }
                   },
                   "runtime": "x64",
                   "loggingLevel": "Basic",
                   "packageLocation": {
                       "packagePath": "FolderName/ProjectName/PackageName.dtsx"
                   },
                   "environmentPath": "FolderName/EnvironmentName",
                   "projectParameters": {
                       "project_param_1": {
                           "value": "123"
                       },
                       "project_param_2": {
                           "value": {
                               "value": "@pipeline().parameters.MyPipelineParameter",
                               "type": "Expression"
                           }
                       }
                   },
                   "packageParameters": {
                       "package_param_1": {
                           "value": "345"
                       },
                       "package_param_2": {
                           "value": {
                               "type": "AzureKeyVaultSecret",
                               "store": {
                                   "referenceName": "myAKV",
                                   "type": "LinkedServiceReference"
                               },
                               "secretName": "MySecret"
                           }
                       }
                   },
                   "projectConnectionManagers": {
                       "MyAdonetCM": {
                           "userName": {
                               "value": "sa"
                           },
                           "passWord": {
                               "value": {
                                   "type": "SecureString",
                                   "value": "abc"
                               }
                           }
                       }
                   },
                   "packageConnectionManagers": {
                       "MyOledbCM": {
                           "userName": {
                               "value": {
                                   "value": "@pipeline().parameters.MyUsername",
                                   "type": "Expression"
                               }
                           },
                           "passWord": {
                               "value": {
                                   "type": "AzureKeyVaultSecret",
                                   "store": {
                                       "referenceName": "myAKV",
                                       "type": "LinkedServiceReference"
                                   },
                                   "secretName": "MyPassword",
                                   "secretVersion": "3a1b74e361bf4ef4a00e47053b872149"
                               }
                           }
                       }
                   },
                   "propertyOverrides": {
                       "\\Package.MaxConcurrentExecutables": {
                           "value": 8,
                           "isSensitive": false
                       }
                   }
               },
               "policy": {
                   "timeout": "0.01:00:00",
                   "retry": 0,
                   "retryIntervalInSeconds": 30
               }
           }]
       }
   }
   ```

2. No Azure PowerShell, mude para a pasta `C:\ADF\RunSSISPackage`.

3. Para criar o pipeline **RunSSISPackagePipeline**, execute o **AzDataFactoryV2Pipeline conjunto** cmdlet.

   ```powershell
   $DFPipeLine = Set-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                  -ResourceGroupName $ResGrp.ResourceGroupName `
                                                  -Name "RunSSISPackagePipeline"
                                                  -DefinitionFile ".\RunSSISPackagePipeline.json"
   ```

   Veja o exemplo de saída:

   ```
   PipelineName      : Adfv2QuickStartPipeline
   ResourceGroupName : <resourceGroupName>
   DataFactoryName   : <dataFactoryName>
   Activities        : {CopyFromBlobToBlob}
   Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
   ```

### <a name="run-the-pipeline"></a>Executar o pipeline
Use o **Invoke-AzDataFactoryV2Pipeline** para executar o pipeline. O cmdlet retorna a ID da execução de pipeline para monitoramento futuro.

```powershell
$RunId = Invoke-AzDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline"></a>Monitorar o Pipeline

Execute o script do PowerShell a seguir para verificar continuamente o status da execução de pipeline até que ela termine de copiar os dados. Copie/cole o script a seguir na janela do PowerShell e pressione ENTER. 

```powershell
while ($True) {
    $Run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName `
                                               -DataFactoryName $DataFactory.DataFactoryName `
                                               -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

Você também pode monitorar o pipeline usando o Portal do Azure. Para obter instruções passo a passo, consulte [Monitorar o pipeline](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

### <a name="schedule-the-pipeline-with-a-trigger"></a>Programar o pipeline com um gatilho
Na etapa anterior, você executou o pipeline sob demanda. Você também pode criar um gatilho de agendamento para a agendar a execução do pipeline (por hora, diariamente, etc.).

1. Crie um arquivo JSON denominado **MyTrigger.json** na pasta **C:\ADF\RunSSISPackage** com o seguinte conteúdo: 

   ```json
   {
       "properties": {
           "name": "MyTrigger",
           "type": "ScheduleTrigger",
           "typeProperties": {
               "recurrence": {
                   "frequency": "Hour",
                   "interval": 1,
                   "startTime": "2017-12-07T00:00:00-08:00",
                   "endTime": "2017-12-08T00:00:00-08:00"
               }
           },
           "pipelines": [{
               "pipelineReference": {
                   "type": "PipelineReference",
                   "referenceName": "RunSSISPackagePipeline"
               },
               "parameters": {}
           }]
       }
   }    
   ```
2. No **Azure PowerShell**, mude para a pasta **C:\ADF\RunSSISPackage**.
3. Execute o **AzDataFactoryV2Trigger conjunto** cmdlet, que cria o gatilho. 

   ```powershell
   Set-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                   -DataFactoryName $DataFactory.DataFactoryName `
                                   -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
   ```
4. Por padrão, o gatilho está no estado interrompido. Inicie o gatilho, executando o **AzDataFactoryV2Trigger início** cmdlet. 

   ```powershell
   Start-AzDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                     -DataFactoryName $DataFactory.DataFactoryName `
                                     -Name "MyTrigger" 
   ```
5. Confirme que o gatilho é iniciado, executando o **Get-AzDataFactoryV2Trigger** cmdlet. 

   ```powershell
   Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                   -DataFactoryName $DataFactoryName `
                                   -Name "MyTrigger"     
   ```    
6. Execute o seguinte comando após a próxima hora. Por exemplo, se a hora atual for 15:25 UTC, execute o comando às 16:00 UTC. 
    
   ```powershell
   Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                      -DataFactoryName $DataFactoryName `
                                      -TriggerName "MyTrigger" `
                                      -TriggerRunStartedAfter "2017-12-06" `
                                      -TriggerRunStartedBefore "2017-12-09"
   ```

   Você pode executar a consulta a seguir no banco de dados SSISDB no seu servidor do SQL Azure para verificar se o pacote foi executado. 

   ```sql
   select * from catalog.executions
   ```

## <a name="next-steps"></a>Próximas etapas
Consulte a postagem blog a seguir:
-   [Modernizar e estender seus fluxos de trabalho ETL/ELT com atividades do SSIS nos pipelines do ADF](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)
