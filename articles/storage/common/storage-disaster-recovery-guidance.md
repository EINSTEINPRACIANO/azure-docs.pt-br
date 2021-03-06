---
title: Recuperação de desastre e failover de conta de armazenamento (versão prévia) – Armazenamento do Azure
description: O armazenamento do Azure dá suporte ao failover de conta (versão prévia) para contas de armazenamento com redundância geográfica. Com o failover de conta, é possível iniciar o processo de failover da conta de armazenamento, se o ponto de extremidade primário ficar indisponível.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/25/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: f9d68af12f6b2e98c77d0bd1b65a82c69588f203
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65147628"
---
# <a name="disaster-recovery-and-storage-account-failover-preview-in-azure-storage"></a>Recuperação de desastre e failover de conta de armazenamento (versão prévia) no Armazenamento do Azure

A Microsoft se empenha em garantir que os serviços do Azure estejam sempre disponíveis. No entanto, podem ocorrer interrupções imprevistas do serviço. Caso seu aplicativo necessite de resiliência, a Microsoft recomenda usar o armazenamento com redundância geográfica para que os dados sejam replicados em uma segunda região. Além disso, os clientes devem dispor de um plano de recuperação de desastre para lidar com uma interrupção regional do serviço. Uma parte importante de um plano de recuperação de desastre é a preparação de um failover para o ponto de extremidade secundário caso o ponto de extremidade primário fique indisponível. 

O armazenamento do Azure dá suporte ao failover de conta (versão prévia) para contas de armazenamento com redundância geográfica. Com o failover de conta, é possível iniciar o processo de failover da conta de armazenamento, se o ponto de extremidade primário ficar indisponível. O failover atualiza o ponto de extremidade secundário para torná-lo um ponto de extremidade primário de sua conta de armazenamento. Quando o failover estiver concluído, os clientes poderão começar a gravar no novo ponto de extremidade primário.

Este artigo descreve os conceitos e o processo envolvidos em um failover de conta e mostra como preparar sua conta de armazenamento para recuperação com o mínimo de impacto para o cliente. Para saber como iniciar um failover de conta no portal do Azure ou no PowerShell, confira o artigo sobre como [iniciar um failover de conta (versão prévia)](storage-initiate-account-failover.md).


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="choose-the-right-redundancy-option"></a>Escolhendo a opção de redundância correta

Todas as contas de armazenamento são replicadas para redundância. A opção de redundância que será escolhida para sua conta dependerá do grau de resiliência que você precisa. Para proteção contra interrupções regionais, escolha o armazenamento com redundância geográfica, com ou sem a opção de acesso de leitura proveniente da região secundária:  

O **Armazenamento com redundância geográfica (GRS)** replica seus dados de forma assíncrona em duas regiões geográficas que estão a, pelo menos, centenas de quilômetros de distância. Se a região primária sofrer uma interrupção, a região secundária funcionará como uma fonte redundante para seus dados. É possível iniciar um failover para transformar o ponto de extremidade secundário no ponto de extremidade primário.

O **Armazenamento com redundância geográfica com acesso de leitura (RA-GRS)** fornece um armazenamento com redundância geográfica com o benefício adicional de acesso de leitura ao ponto de extremidade secundário. Se ocorrer uma interrupção no ponto de extremidade primário, os aplicativos configurados para o RA-GRS e projetados para alta disponibilidade poderão continuar a ler os dados a partir do ponto de extremidade secundário. A Microsoft recomenda o RA-GRS para garantir a máxima resiliência de seus aplicativos.

Outras opções de redundância de armazenamento do Azure incluem o armazenamento com redundância de zona (ZRS), que replica seus dados entre zonas de disponibilidade em uma única região e o armazenamento localmente redundante (LRS), que replica seus dados em um único data center em uma única região. Se sua conta de armazenamento for configurada para o ZRS ou o LRS, será possível converter essa conta para usar o GRS ou o RA-GRS. Configurar sua conta para o armazenamento com redundância geográfica acarretará custos adicionais. Para obter mais informações, consulte [Replicação do Armazenamento do Azure](storage-redundancy.md).

> [!WARNING]
> O Armazenamento com redundância geográfica envolve risco de perda de dados. Os dados são replicados para a região secundária de forma assíncrona, o que significa que há um atraso entre o momento em que os dados são gravados na região primária e o momento em que são gravados na região secundária. Caso ocorra uma interrupção, as operações de gravação no ponto de extremidade primário que ainda não tenham sido replicadas no ponto de extremidade secundário serão perdidas. 

## <a name="design-for-high-availability"></a>Projetando para a alta disponibilidade

É importante projetar seu aplicativo para ter uma alta disponibilidade desde o início. Consulte estes recursos do Azure para obter orientações sobre como projetar seu aplicativo e sobre o planejamento para a recuperação de desastre:

* [Projetando aplicativos resilientes para o Azure](https://docs.microsoft.com/azure/architecture/resiliency/): uma visão geral dos principais conceitos de arquitetura para aplicativos altamente disponíveis no Azure.
* [Lista de verificação de disponibilidade](https://docs.microsoft.com/azure/architecture/checklist/availability): uma lista de verificação para analisar se seu aplicativo implementa as práticas de design recomendadas para ter uma alta disponibilidade.
* [Projetando aplicativos altamente disponíveis usando o RA-GRS](storage-designing-ha-apps-with-ragrs.md): diretrizes de design para a criação de aplicativos para aproveitar o RA-GRS.
* [Tutorial: criar um aplicativo altamente disponível com o armazenamento de Blob](../blobs/storage-create-geo-redundant-storage.md): um tutorial que mostra como criar um aplicativo altamente disponível que alterne automaticamente entre os pontos de extremidade à medida que falhas e recuperações são simuladas. 

Além disso, tenha em mente essas práticas recomendadas a fim de manter a alta disponibilidade de seus dados de armazenamento no Azure:

* **Discos:** Use o [Backup do Azure](https://azure.microsoft.com/services/backup/) para fazer o backup dos discos de VM usados pelas máquinas virtuais do Azure. Considere também usar o [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) para proteger suas VMs em caso de desastre regional.
* **Blobs de blocos:** ative a [exclusão reversível](../blobs/storage-blob-soft-delete.md) para proteger-se de exclusões de nível de objeto e substituições ou para copiar os blobs para outra conta de armazenamento em uma região diferente usando o [AzCopy](storage-use-azcopy.md), o [Azure PowerShell](storage-powershell-guide-full.md) ou a [Biblioteca de Movimentação de Dados do Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* **Arquivos:** use o [AzCopy](storage-use-azcopy.md) ou o [Azure PowerShell](storage-powershell-guide-full.md) para copiar os arquivos para outra conta de armazenamento em uma região diferente.
* **Tabelas:** use o [AzCopy](storage-use-azcopy.md) para exportar os dados de tabela para outra conta de armazenamento em uma região diferente.

## <a name="track-outages"></a>Acompanhar interrupções

Os clientes podem assinar o [Painel de Integridade do Serviço do Azure](https://azure.microsoft.com/status/) para acompanhar a integridade e o status do armazenamento do Azure e de outros serviços do Azure.

A Microsoft também recomenda que você projete seu aplicativo de forma a se preparar para a possibilidade de falhas de gravação. Seu aplicativo deve expor falhas de gravação de forma que o alerte para a possibilidade de uma interrupção na região primária.

## <a name="understand-the-account-failover-process"></a>Entendendo o processo de failover de conta

O failover de conta gerenciada pelo cliente (versão prévia) permite fazer failover da conta de armazenamento inteira para a região secundária se a primária ficar indisponível por algum motivo. Quando você força um failover para a região secundária, os clientes podem começar a gravar os dados no ponto de extremidade secundário depois que o failover estiver concluído. O failover normalmente leva cerca de uma hora.

### <a name="how-an-account-failover-works"></a>Como o failover de conta funciona

Em circunstâncias normais, um cliente grava os dados em uma conta de armazenamento do Azure na região primária, e os dados são replicados de forma assíncrona na região secundária. A imagem a seguir mostra o cenário quando a região primária está disponível:

![Os clientes gravam os dados na conta de armazenamento na região primária](media/storage-disaster-recovery-guidance/primary-available.png)

Se o ponto de extremidade primário ficar indisponível por algum motivo, o cliente não poderá mais gravar os dados na conta de armazenamento. A imagem a seguir mostra o cenário em que o ponto de extremidade primário ficou indisponível, mas nenhuma recuperação aconteceu ainda:

![O ponto de extremidade primário está indisponível, portanto, os clientes não podem gravar dados](media/storage-disaster-recovery-guidance/primary-unavailable-before-failover.png)

O cliente inicia o failover da conta no ponto de extremidade secundário. O processo de failover atualiza a entrada DNS fornecida pelo armazenamento do Azure para que o ponto de extremidade secundário se torne o novo ponto de extremidade primário de sua conta de armazenamento, conforme mostrado na imagem a seguir:

![O cliente inicia o failover da conta no ponto de extremidade secundário](media/storage-disaster-recovery-guidance/failover-to-secondary.png)

O acesso à gravação é restaurado nas contas de GRS e de RA-GRS depois que a entrada DNS for atualizada e as solicitações estejam sendo direcionadas para o novo ponto de extremidade primário. Os pontos de extremidade de armazenamento existentes para blobs, tabelas, filas e arquivos permanecem os mesmos após o failover.

> [!IMPORTANT]
> Depois que o failover estiver concluído, a conta de armazenamento é configurada para ser localmente redundante no novo ponto de extremidade primário. Para continuar a replicação no novo ponto de extremidade secundário, configure a conta para usar o armazenamento com redundância geográfica novamente (RA-GRS ou GRS).
>
> Tenha em mente que a conversão de uma conta de LRS para RA-GRS ou GRS tem um custo. Esse custo se aplica à atualização da conta de armazenamento na nova região primária para usar o RA-GRS ou o GRS após um failover.  

### <a name="anticipate-data-loss"></a>Prevendo a perda de dados

> [!CAUTION]
> Um failover de conta geralmente envolve alguma perda de dados. É importante entender as implicações de iniciar um failover de conta.  

Como os dados são gravados de forma assíncrona da região primária para a região secundária, haverá sempre um atraso antes de uma gravação na região primária ser replicada na região secundária. Se a região primária ficar indisponível, as gravações mais recentes poderão não terem ainda sido replicadas na região secundária.

Ao forçar um failover, todos os dados na região primária são perdidos uma vez que a região secundária se torna a nova região primária e a conta de armazenamento é configurada para ser localmente redundante. Todos os dados já replicados na região secundária são mantidos quando ocorre o failover. No entanto, os dados gravados na região primária que ainda não tenham sido replicados na região secundária serão perdidos permanentemente. 

A propriedade **Hora da última sincronização** indica o momento mais recente em que os dados da região primária foram certamente gravados na região secundária. Todos os dados gravados antes da hora da última sincronização estarão disponíveis na região secundária, enquanto os dados gravados após esse momento podem não terem sido gravados na região secundária e terem sido perdidos. Use essa propriedade caso ocorra uma interrupção para estimar a quantidade de perda de dados que poderá incorrer ao iniciar um failover de conta. 

Como prática recomendada, projete seu aplicativo de modo que seja possível usar a hora da última sincronização para avaliar a estimativa de perda de dados. Por exemplo, se você estiver registrando todas as operações de gravação, será possível comparar o momento das últimas operações de gravação com a hora da última sincronização a fim de determinar quais gravações não foram sincronizadas na região secundária.

### <a name="use-caution-when-failing-back-to-the-original-primary"></a>Tendo cuidados ao realizar o fail back para a região primária original

Após realizar o failover da região primária para a região secundária, a conta de armazenamento estará configurada para ser localmente redundante na nova região primária. É possível configurar a conta para a redundância geográfica novamente, atualizando-a para usar o GRS ou o RA-GRS. Quando a conta é novamente configurada para a redundância geográfica, após um failover, a nova região primária imediatamente começa a replicar os dados na nova região secundária, que era a primária antes do failover original. No entanto, pode levar um certo tempo para que os dados existentes na região primária sejam totalmente replicados na nova região secundária.

Depois que a conta de armazenamento for reconfigurada para a redundância geográfica, será possível iniciar outro failover da nova região primária de volta para a nova região secundária. Nesse caso, a região primária original antes do failover se tornará novamente a região primária e estará configurada para ser localmente redundante. Todos os dados na região primária após o failover (a região secundária original) serão perdidos. Se a maioria dos dados na conta de armazenamento ainda não foi replicada para a nova região secundária antes do fail back, é possível que você tenha uma perda grande de dados. 

Para evitar uma grande perda de dados, verifique o valor da propriedade **Hora da última sincronização** antes de realizar o fail back. Compare a hora da última sincronização com os momentos das últimas vezes que os dados foram gravados na nova região primária a fim de avaliar a perda de dados esperada. 

## <a name="initiate-an-account-failover"></a>Iniciar um failover da conta

É possível iniciar um failover de conta do portal do Azure, no PowerShell, na CLI do Azure ou na API de provedor de recursos do Armazenamento do Azure. Para obter mais informações sobre como iniciar um failover, confira o artigo sobre como [iniciar um failover de conta (versão prévia)](storage-initiate-account-failover.md).

## <a name="about-the-preview"></a>Sobre a visualização

O failover de conta está disponível em versão prévia para todos os clientes que utilizam o GRS ou o RA-GRS com implantações do Azure Resource Manager. Há suporte para uso geral v1, uso geral v2 e os tipos de conta de armazenamento de Blob. O failover de conta está disponível atualmente nestas regiões:

- Oeste dos EUA 2
- Centro-Oeste dos EUA

A versão prévia é destinada apenas para uso não produtivo. SLAs (Contratos de Nível de Serviço) não estão disponíveis atualmente.

### <a name="register-for-the-preview"></a>Registre-se para a versão prévia

Para se registrar para a versão prévia, execute os seguintes comandos no PowerShell. Certifique-se de substituir o espaço reservado entre colchetes pela sua própria ID de assinatura:

```powershell
Connect-AzAccount -SubscriptionId <subscription-id>
Register-AzProviderFeature -FeatureName CustomerControlledFailover -ProviderNamespace Microsoft.Storage
```

Pode levar de um a dois dias para receber a aprovação para a versão prévia. Para verificar que seu registro foi aprovado, execute o seguinte comando:

```powershell
Get-AzProviderFeature -FeatureName CustomerControlledFailover -ProviderNamespace Microsoft.Storage
```

### <a name="additional-considerations"></a>Considerações adicionais 

Examine as considerações adicionais descritas nesta seção para entender como seus aplicativos e serviços podem ser afetados quando você força um failover durante o período da versão prévia.

#### <a name="azure-virtual-machines"></a>Máquinas virtuais do Azure

As máquinas virtuais (VMs) do Azure não realizarão o failover como parte de um failover de conta. Se a região primária ficar indisponível, e você fizer o failover para a região secundária, será preciso recriar todas as VMs após o failover. 

#### <a name="azure-unmanaged-disks"></a>Discos não gerenciados do Azure

Como prática recomendada, a Microsoft aconselha converter os discos não gerenciados em discos gerenciados. No entanto, se você precisar fazer o failover em uma conta que contenha discos não gerenciados anexados a VMs do Azure, será necessário desligar a VM antes de iniciar o failover.

Os discos não gerenciados são armazenadas como blobs de páginas no Armazenamento do Azure. Quando uma VM está em execução no Azure, todos os discos não gerenciados anexados à VM são concedidos. O failover de conta não poderá continuar se houver uma concessão em um blob. Para realizar o failover, siga estas etapas:

1. Antes de começar, anote os nomes de todos os discos não gerenciados, seus números de unidade lógica (LUN) e a VM à qual eles estão anexados. Isso facilitará anexar os discos novamente após o failover. 
2. Desligue a VM.
3. Exclua a VM, mas mantenha os arquivos VHD para os discos não gerenciados. Anote a hora que você excluiu a VM.
4. Aguarde até que a **Hora da última sincronização** seja atualizada, ela deve posterior à hora em que você excluiu a VM. Esta etapa é importante porque se o ponto de extremidade secundário não foi totalmente atualizado com os arquivos VHD quando o failover ocorrer, a VM poderá não funcionar corretamente na nova região primária.
5. Inicie o failover da conta.
6. Aguarde até que o failover da conta esteja concluído e a região secundária tenha se tornado a nova região primária.
7. Crie uma VM na nova região primária e anexe novamente os VHDs.
8. Inicie a nova VM.

Tenha em mente que todos os dados armazenados em um disco temporário serão perdidos quando a VM for desligada.

### <a name="unsupported-features-or-services"></a>Recursos ou serviços sem suporte
Não há suporte aos seguintes recursos ou serviços para o failover de conta na versão prévia:

- A Sincronização de Arquivos do Azure não oferece suporte ao failover de conta de armazenamento. Não deve ser realizado o failover das contas de armazenamento que contêm compartilhamentos de arquivos do Azure que estejam sendo usadas como pontos de extremidade de nuvem na Sincronização de Arquivos do Azure. Se isso for feito, a sincronização deixará de funcionar e poderá causar a perda inesperada de dados no caso de arquivos recentes em camadas.  
- Não é possível realizar failover nas contas de armazenamento usando o namespace hierárquico do Azure Data Lake Storage Gen2.
- Não é possível fazer failover em uma conta de armazenamento que contenha blobs. Mantenha os blobs arquivados em uma conta de armazenamento separada na qual não planeje fazer failover.
- Não é possível realizar failover em uma conta de armazenamento que contenha blob de blocos premium. As contas de armazenamento que dão suporte a blobs de bloco premium atualmente não são compatíveis com a redundância geográfica.
- Após o failover estiver concluído os recursos a seguir deixará de funcionar se originalmente habilitado: [As assinaturas de eventos](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-overview), [políticas de ciclo de vida](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts), [o log de análise de armazenamento](https://docs.microsoft.com/rest/api/storageservices/about-storage-analytics-logging).

## <a name="copying-data-as-an-alternative-to-failover"></a>Copiando dados como uma alternativa ao failover

Se sua conta de armazenamento estiver configurada para o RA-GRS, você terá acesso de leitura aos seus dados usando o ponto de extremidade secundário. Se você preferir não realizar o failover no caso de uma interrupção na região primária, será possível usar ferramentas como o [AzCopy](storage-use-azcopy.md), o [Azure PowerShell](storage-powershell-guide-full.md) ou a [Biblioteca de movimentação de dados do Azure](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) para copiar os dados de sua conta de armazenamento na região secundária para outra conta de armazenamento em uma região não afetada. Você poderá, então, direcionar os aplicativos para essa conta de armazenamento para a disponibilidade de leitura e de gravação.

## <a name="microsoft-managed-failover"></a>Failover gerenciado pela Microsoft

Em circunstâncias extremas em que uma região for perdida devido a um desastre significativo, a Microsoft poderá iniciar um failover regional. Nesse caso, nenhuma ação sua é necessária. Você não terá acesso para gravação na conta de armazenamento até que o failover gerenciado pela Microsoft seja concluído. Seus aplicativos poderão ler a partir da região secundária se sua conta de armazenamento estiver configurada para o RA-GRS. 

## <a name="see-also"></a>Consulte também

* [Iniciar um failover de conta (versão prévia)](storage-initiate-account-failover.md)
* [Criando aplicativos altamente disponíveis usando RA-GRS](storage-designing-ha-apps-with-ragrs.md)
* [Tutorial: Criar um aplicativo altamente disponível com o armazenamento de Blobs](../blobs/storage-create-geo-redundant-storage.md) 
