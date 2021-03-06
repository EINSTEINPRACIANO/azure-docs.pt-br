---
title: Proteger novamente VMs do Azure que sofreram failover de volta para a região do Azure primária com o Azure Site Recovery | Microsoft Docs
description: Descreve como proteger novamente VMs do Azure em uma região secundária, após o failover de uma região primária, usando o Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: eabb7d194a3ef65282befab1ae59e85ba56f2f5b
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472152"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Proteger novamente VMs do Azure que sofreram failover para a região primária


Quando você [faz failover](site-recovery-failover.md) de VMs do Azure de uma região para outra usando o [Azure Site Recovery](site-recovery-overview.md), as VMs inicializam na região secundária, em um estado desprotegido. Se você fizer failback das VMs de volta para a região primária, faça o seguinte:

- Proteja novamente as VMs na região secundária, para que elas iniciem a replicação para a região primária.
- Depois que a nova proteção é concluída e as VMs estão replicando, você pode fazer failover delas da região secundária para a primária.

## <a name="prerequisites"></a>Pré-requisitos
1. O failover de VM da região primária para a secundária deve ser confirmado.
2. O local de destino primário deve estar disponível e você deve ser capaz de acessar ou criar recursos nessa região.

## <a name="reprotect-a-vm"></a>Proteger novamente uma VM

1. Em **Cofre** > **Itens replicados**, clique com o botão direito do mouse na VM em que o failover foi executado e, em seguida, selecione **Proteger Novamente**. A direção de nova proteção deve mostrar da secundária para a primária.

   ![Proteger novamente](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Examine o grupo de recursos, a rede, o armazenamento e os conjuntos de disponibilidade. Em seguida, clique em **OK**. Se houver algum recurso marcado como novo, ele será criado como parte do processo de nova proteção.
3. O trabalho de nova proteção propaga os dados mais recentes para o site de destino. Depois que isso termina, ocorre a replicação delta. Em seguida, você pode fazer failover de volta para o site primário. Você pode selecionar a conta de armazenamento ou a rede que você deseja usar ao proteger novamente, usando a opção personalizar.

   ![Opção de personalização](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Personalizar as configurações de nova proteção

Você pode personalizar as propriedades a seguir da VM de destino ao proteger novamente.

![Personalizar](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Propriedade |Observações  |
|---------|---------|
|Grupo de recursos de destino     | Modifique o grupo de recursos de destino no qual a VM é criada. Como parte da nova proteção, a VM de destino é excluída. Você pode escolher um novo grupo de recursos sob o qual criar a VM após o failover.        |
|Rede virtual de destino     | A rede de destino não pode ser alterada durante o trabalho de nova proteção. Para alterar a rede, refaça o mapeamento de rede.         |
|Armazenamento de destino (a VM secundária não usa discos gerenciados)     | Você pode alterar a conta de armazenamento que a VM usa após o failover.         |
|Discos gerenciados de réplica (a VM secundária usa discos gerenciados)    | O Site Recovery cria discos gerenciados de réplica na região primária para espelhar discos gerenciados da VM secundária.         |
|Armazenamento de cache     | Você pode especificar uma conta de armazenamento de cache a ser usada durante a replicação. Por padrão, uma nova conta de armazenamento de cache será criada se ela não existir.         |
|Conjunto de disponibilidade     |Se a VM na região secundária for parte de um conjunto de disponibilidade, você poderá escolher um conjunto de disponibilidade para a VM de destino na região primária. Por padrão, o Site Recovery tenta localizar o conjunto de disponibilidade existente na região primária e usá-lo. Durante a personalização, você pode especificar um novo conjunto de disponibilidade.         |


### <a name="what-happens-during-reprotection"></a>O que acontece ao proteger novamente?

Por padrão, acontece o seguinte:

1. Uma conta de armazenamento de cache é criada na região onde a VM com failover está em execução.
2. Se a conta de armazenamento de destino (a conta de armazenamento original na região primária) não existir, uma nova será criada. O nome da conta de armazenamento atribuído é o nome da conta de armazenamento usada pela VM secundária, com o sufixo "asr".
3. Se a sua VM usar discos gerenciados, os discos gerenciados de réplica serão criados na região primária para armazenar os dados replicados dos discos da VM secundária.
4. Se o conjunto de disponibilidade de destino não existir, um novo será criado como parte do trabalho de nova proteção, se necessário. Se você personalizar as configurações de nova proteção, o conjunto selecionado será usado.

Quando você dispara um trabalho de nova proteção e a VM de destino existe, ocorre o seguinte:

1. A VM do lado de destino é desligada quando ela está em execução.
2. Se a VM estiver usando discos gerenciados, uma cópia dos discos originais serão criados com o sufixo '-ASRReplica'. Os discos originais são excluídos. As cópias '-ASRReplica' são usadas para replicação.
3. Se a VM estiver usando discos não gerenciados, os discos de dados da VM de destino são desanexados e usados para replicação. Uma cópia do disco do sistema operacional é criada e anexada na VM. O disco do sistema operacional original é desanexado e usado para replicação.
4. Somente alterações entre o disco de origem e o disco de destino são sincronizadas. Os diferenciais são computados comparando ambos os discos e, em seguida, transferidos. Para localizar a verificação de tempo estimado abaixo.
5. Após a conclusão da sincronização, a replicação delta começa e cria um ponto de recuperação de acordo com a política de replicação.

Quando você dispara um trabalho de nova proteção e a VM de destino e o disco não existem, ocorre o seguinte:
1. Se a VM estiver usando discos gerenciados, réplicas dos discos originais serão criadas com o sufixo '-ASRReplica'. As cópias '-ASRReplica' são usadas para replicação.
2. Se a VM estiver usando discos não gerenciados, réplicas dos discos originais serão criadas na conta de armazenamento de destino.
3. Todos discos são copiados da região com failover para a nova região de destino.
4. Após a conclusão da sincronização, a replicação delta começa e cria um ponto de recuperação de acordo com a política de replicação.

#### <a name="estimated-time--to-do-the-reprotection"></a>Tempo estimado para fazer a nova proteção 

Na maioria dos casos, o Azure Site Recovery não replica os dados completos para a região de origem. Abaixo estão as condições que determina a quantidade de dados será replicada:

1.  Se a fonte de dados da VM for excluído, corrompido ou inacessível por alguma razão, como o grupo de recursos altera/exclui, em seguida, durante a IR completo da nova proteção acontecerá, pois não há nenhum dado disponível na região de origem para usar.
2.  Se a fonte de dados da VM está acessível apenas diferenciais são calculados comparando dois discos e, então, transferidos. Verifique a tabela a seguir para obter o tempo estimado 

|* * Situação de exemplo * * | * * Tempo necessário para a nova proteção * * |
|--- | --- |
|Região de origem tem 1 VM com 1 TB de disco padrão<br/>-Somente os dados de 127 GB são usados e o restante do disco está vazio<br/>-Tipo de disco é o padrão com taxa de transferência 60 MiB/S<br/>-Nenhuma alteração de dados após o failover| Tempo aproximado 45 minutos – 1,5 horas<br/> -Durante a nova proteção, recuperação de Site preencherá a soma de verificação de dados inteiros, que o levará a 127 GB / 45 MBs aproximadamente 45 minutos<br/>-Algum tempo de sobrecarga é necessário para o Site Recovery automaticamente a escala é de 20 a 30 minutos<br/>-Sem nenhum encargo |
|Região de origem tem 1 VM com 1 TB de disco padrão<br/>-Somente os dados de 127 GB são usados e o restante do disco está vazio<br/>-Tipo de disco é o padrão com taxa de transferência 60 MiB/S<br/>-As alterações de dados 45 GB após o failover| Tempo aproximado horas 1 – 2 horas<br/>-Durante a nova proteção, recuperação de Site preencherá a soma de verificação de dados inteiros, que o levará a 127 GB / 45 MBs aproximadamente 45 minutos<br/>-Tempo para aplicar as alterações de 45 GB é 45 GB de transferência / 45 MBps ~ 17 minutos<br/>-Encargos de saída seria apenas para 45 GB de dados não para a soma de verificação|
 



## <a name="next-steps"></a>Próximas etapas

Depois que a VM estiver protegida, você poderá iniciar um failover. O failover desliga a VM na região secundária e cria e inicializa a VM na região primária, com um pequeno tempo de inatividade. Recomendamos que você escolha um horário adequadamente e que execute um failover de teste, mas iniciando um failover completo para o site primário. [Saiba mais](site-recovery-failover.md) sobre failover.
