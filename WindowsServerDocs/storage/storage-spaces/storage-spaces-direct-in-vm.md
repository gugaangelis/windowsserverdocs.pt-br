---
title: Usando Espaços de Armazenamento Diretos em uma máquina virtual
ms.prod: windows-server
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Como implantar Espaços de Armazenamento Diretos em um cluster de convidado de máquina virtual, por exemplo, em Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: ab0ce792c5a948e763a48493a78ccdac7a6fe74c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366053"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>Usando Espaços de Armazenamento Diretos em clusters de máquinas virtuais convidadas

> Aplica-se a: Windows Server 2019, Windows Server 2016

Você pode implantar Espaços de Armazenamento Diretos em um cluster de servidores físicos ou em clusters convidados da máquina virtual, conforme discutido neste tópico. Esse tipo de implantação fornece armazenamento compartilhado virtual em um conjunto de VMs sobre uma nuvem privada ou pública para que as soluções de alta disponibilidade do aplicativo possam ser usadas para aumentar a disponibilidade dos aplicativos.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>Implantando em clusters convidados da VM IaaS do Azure

Os [modelos do Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) foram publicados diminuem a complexidade, configuram as práticas recomendadas e a velocidade de suas implantações de espaços de armazenamento diretos em uma VM IaaS do Azure. Esta é a solução recomendada para a implantação no Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>Requisitos

As considerações a seguir se aplicam ao implantar Espaços de Armazenamento Diretos em um ambiente virtualizado.

> [!TIP]
> Os modelos do Azure configurarão automaticamente as considerações abaixo para você e serão a solução recomendada durante a implantação em VMs IaaS do Azure.

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

-   Desabilite os recursos de substituição automática de unidade no Serviço de Integridade executando o seguinte cmdlet do PowerShell:

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   Sem suporte: Instantâneo/restauração de disco virtual de nível de host

    Em vez disso, use soluções de backup de nível convidado tradicionais para fazer backup e restaurar os dados nos volumes Espaços de Armazenamento Diretos.

-   Para proporcionar maior resiliência a uma possível latência de armazenamento VHD/VHDX/VMDK em clusters convidados, aumente o valor de tempo limite de e/s de espaços de armazenamento:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    O equivalente Decimal de hexadecimal 7530 é 30000, que é de 30 segundos. Observe que o valor padrão é 1770 hexadecimal ou 6000 decimal, que é de 6 segundos.

## <a name="see-also"></a>Consulte também

[Modelos adicionais de VM IaaS do Azure para implantar espaços de armazenamento diretos, vídeos e guias passo a passo](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

[Visão geral Espaços de Armazenamento Diretos adicional](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
