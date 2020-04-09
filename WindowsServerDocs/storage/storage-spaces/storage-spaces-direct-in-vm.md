---
title: Usando Espaços de Armazenamento Diretos em uma máquina virtual
ms.prod: windows-server
ms.author: eldenc
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Como implantar Espaços de Armazenamento Diretos em um cluster de convidado de máquina virtual, por exemplo, em Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: 74b1b90a780a0b238a356e942f8348e2a483d94a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856109"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>Usando Espaços de Armazenamento Diretos em clusters de máquinas virtuais convidadas

> Aplica-se a: Windows Server 2019, Windows Server 2016

Você pode implantar Espaços de Armazenamento Diretos em um cluster de servidores físicos ou em clusters convidados da máquina virtual, conforme discutido neste tópico. Esse tipo de implantação fornece armazenamento compartilhado virtual em um conjunto de VMs sobre uma nuvem privada ou pública para que as soluções de alta disponibilidade do aplicativo possam ser usadas para aumentar a disponibilidade dos aplicativos.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>Implantando em clusters convidados da VM IaaS do Azure

Os [modelos do Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) foram publicados diminuem a complexidade, configuram as práticas recomendadas e a velocidade de suas implantações de espaços de armazenamento diretos em uma VM IaaS do Azure. Esta é a solução recomendada para a implantação no Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>{1&gt;{2&gt;Requisitos&lt;2}&lt;1}

As considerações a seguir se aplicam ao implantar Espaços de Armazenamento Diretos em um ambiente virtualizado.
       
>        !TIP]
>        zure templates will automatically configure the below considerations for you and are the recommended solution when deploying in Azure IaaS VMs.

-   Mínimo de 2 nós e máximo de 3 nós

-   implantações de 2 nós devem configurar uma testemunha (testemunha de nuvem ou testemunha de compartilhamento de arquivos)

-   as implantações de 3 nós podem tolerar um nó inativo e a perda de 1 ou mais discos em outro nó.  Se 2 nós forem desligados, os discos virtuais estarão offline até que um dos nós seja retornado.  

-   Configurar as máquinas virtuais a serem implantadas entre domínios de falha

    -   Azure – configurar conjunto de disponibilidade

    -   Hyper-V – configurar o AntiAffinityClassNames nas VMs para separar as VMs entre os nós

    -   VMware – configure a regra de antiafinidade da VM VM criando uma regra de DRS do tipo ' máquinas virtuais separadas ' para separar as VMs entre hosts ESX. Os discos apresentados para uso com Espaços de Armazenamento Diretos devem usar o adaptador paravirtual SCSI (PVSCSI). Para obter suporte do PVSCSI com o Windows Server, consulte https://kb.vmware.com/s/article/1010398.

-   Aproveite o armazenamento de baixa latência/alto desempenho – os discos gerenciados do armazenamento Premium do Azure são necessários

-   Implantar um design de armazenamento simples sem dispositivos de cache configurados

-   Mínimo de dois discos de dados virtuais apresentados a cada VM (VHD/VHDX/VMDK)

    Esse número é diferente de implantações bare-metal porque os discos virtuais podem ser implementados como arquivos que não são suscetíveis a falhas físicas.

-   Desabilite a substituição automática da unidade "apab" lities no Serviço de Integridade executando o seguinte cmdlet do PowerShell:

    ```powershell
          Get-storagesubsystem clus* | set-storagehealthsetting -name "System.Storage.PhysicalDisk.AutoReplace.Enabled" -value "False"
          ```

-   To give greater resiliency to possible VHD / VHDX / VMDK storage latency in guest clusters, increase the Storage Spaces I/O timeout value:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    The decimal equivalent of Hexadecimal 7530 is 30000, which is 30 seconds. Note that the default value is 1770 Hexadecimal, or 6000 Decimal, which is 6 seconds.

## Not supported

-   Host level virtual disk snapshot/restore

    Instead use traditional guest level backup solutions to backup and restore the data on the Storage Spaces Direct volumes.

-   Host level virtual disk size change

    The virtual disks exposed through the virtual machine must retain the same size and characteristics. Adding more capacity to the storage pool can be accomplished by adding more virtual disks to each of the virtual machines and adding them to the pool. It's highly recommended to use virtual disks of the same size and characteristics as the current virtual disks.

## See also

[Additional Azure Iaas VM templates for deploying Storage Spaces Direct, videos, and step-by-step guides](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

[Additional Storage Spaces Direct Overview](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
""""""''''                                                                                                                                                                        """"""''''                                                                                                                                                                        