---
title: Como redefinir o adaptador de rede de uma VM Windows do Azure | Microsoft Docs
description: Mostra como redefinir o adaptador de rede de uma VM Windows do Azure
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 11/16/2018
ms.author: genli
ms.openlocfilehash: 3a8e005f8678deef9fc4aebd2d620619fe6074bc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60307270"
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a>Como redefinir o adaptador de rede de uma VM Windows do Azure 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Este artigo mostra como redefinir o adaptador de rede para uma VM do Windows no Azure para resolver problemas quando você não pode se conectar à VM (Máquina Virtual) do Windows no Microsoft Azure após:

* A desabilitação da NIC (adaptador de rede) padrão. 
* A definição manual de um IP estático para a NIC. 

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="reset-network-interface"></a>Redefinir o adaptador de rede

### <a name="for-vms-deployed-in-resource-group-model"></a>Para as VMs implantadas no modelo de Grupo de recursos

1.  Vá para o [Portal do Azure](https://ms.portal.azure.com).
2.  Selecione a Máquina Virtual afetada.
3.  Selecione **Rede** e, em seguida, selecione o Adaptador de rede da VM.

    ![Localização do adaptador de rede](./media/reset-network-interface/select-network-interface-vm.png)
    
4.  Selecione **Configurações de IP**.
5.  Selecione o IP. 
6.  Se a **Atribuição de IP privado** não for **Estática**, altere-a para **Estática**.
7.  Altere o **endereço IP** para outro endereço IP que está disponível na Sub-rede.
8. A máquina virtual será reiniciada para inicializar a nova NIC no sistema.
9.  Tente executar o RDP no computador. Se for bem-sucedido, você poderá alterar o endereço IP Privado novamente para o original, se desejar. Caso contrário, você poderá mantê-lo. 

#### <a name="use-azure-powershell"></a>Usar PowerShell do Azure

1. Verifique se você tem [o mais recente Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) instalado
2. Abra uma sessão privilegiada do Azure PowerShell (Executar como administrador). Execute os seguintes comandos:

    ```powershell
    #Set the variables 
    $SubscriptionID = "<Subscription ID>"
    $VM = "<VM Name>"
    $ResourceGroup = "<Resource Group>"
    $VNET = "<Virtual Network>"
    $IP = "NEWIP"

    #Log in to the subscription 
    Add-AzAccount
    Select-AzSubscription -SubscriptionId $SubscriptionId 
    
    #Check whether the new IP address is available in the virtual network.
    Test-AzureStaticVNetIP –VNetName $VNET –IPAddress  $IP

    #Add/Change static IP. This process will not change MAC address
    Get-AzVM -ServiceName $ResourceGroup -Name $VM | Set-AzureStaticVNetIP -IPAddress $IP | Update-AzVM
    ```
3. Tente executar o RDP no computador.  Se for bem-sucedido, você poderá alterar o endereço IP Privado novamente para o original, se desejar. Caso contrário, você poderá mantê-lo.

### <a name="for-classic-vms"></a>Para VMs Clássicas

Para redefinir o adaptador de rede, siga estas etapas:

#### <a name="use-azure-portal"></a>Usar o portal do Azure

1.  Vá para o [Portal do Azure]( https://ms.portal.azure.com).
2.  Selecione **Máquinas Virtuais (Clássicas)**.
3.  Selecione a Máquina Virtual afetada.
4.  Selecione **Endereços IP**.
5.  Se a **Atribuição de IP privado** não for **Estática**, altere-a para **Estática**.
6.  Altere o **endereço IP** para outro endereço IP que está disponível na Sub-rede.
7.  Clique em **Salvar**.
8.  A máquina virtual será reiniciada para inicializar a nova NIC no sistema.
9.  Tente executar o RDP no computador. Se for bem-sucedido, você poderá optar por reverter o Endereço IP privado para o original.  

#### <a name="use-azure-powershell"></a>Usar PowerShell do Azure

1. Certifique-se de que você tenha o [Azure PowerShell mais recente](https://docs.microsoft.com/powershell/azure/overview) instalado.
2. Abra uma sessão privilegiada do Azure PowerShell (Executar como administrador). Execute os seguintes comandos:

    ```powershell
    #Set the variables 
    $SubscriptionID = "<Subscription ID>"
    $VM = "<VM Name>"
    $CloudService = "<Cloud Service>"
    $VNET = "<Virtual Network>"
    $IP = "NEWIP"

    #Log in to the subscription 
    Add-AzureAccount
    Select-AzureSubscription -SubscriptionId $SubscriptionId 

    #Check whether the new IP address is available in the virtual network.
    Test-AzureStaticVNetIP –VNetName $VNET –IPAddress  $IP
    
    #Add/Change static IP. This process will not change MAC address
    Get-AzureVM -ServiceName $CloudService -Name $VM | Set-AzureStaticVNetIP -IPAddress $IP |Update-AzureVM
    ```
3. Tente executar o RDP no computador. Se for bem-sucedido, você poderá alterar o endereço IP Privado novamente para o original, se desejar. Caso contrário, você poderá mantê-lo. 

## <a name="delete-the-unavailable-nics"></a>Excluir as NICs não disponíveis
Depois que você conseguir conectar a área de trabalho remota ao computador, deverá excluir as NICs antigas para evitar o problema potencial:

1.  Abra o Gerenciador de Dispositivos.
2.  Selecione **Exibir** > **Mostrar dispositivos ocultos**.
3.  Selecione **Adaptadores de Rede**. 
4.  Verifique os adaptadores nomeados como “Adaptador de Rede do Microsoft Hyper-V”.
5.  Talvez você veja um adaptador não disponível que está esmaecido. Clique com o botão direito do mouse no adaptador e, em seguida, selecione Desinstalar.

    ![a imagem da NIC](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Desinstale somente os adaptadores não disponíveis que têm o nome “Adaptador de Rede do Microsoft Hyper-V”. Se você desinstalar um dos outros adaptadores ocultos, isso poderá causar outros problemas.
    >
    >

6.  Agora, todos os adaptadores não disponíveis deverão ser limpos do sistema.
