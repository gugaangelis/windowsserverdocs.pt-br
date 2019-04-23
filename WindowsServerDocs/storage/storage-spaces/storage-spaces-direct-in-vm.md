---
title: Usando espaços de armazenamento diretos em uma máquina virtual
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Como implantar espaços de armazenamento diretos em um cluster de convidado da máquina virtual - por exemplo, no Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: a741475d09ab7f7ab29f158ef55378ca9a37beac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841017"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>Usando espaços de armazenamento diretos em clusters convidados da máquina virtual

> Aplica-se a: Windows Server 2019, Windows Server 2016

Você pode implantar espaços de armazenamento diretos em um cluster de servidores físicos ou em clusters de convidado de máquina virtual, conforme discutido neste tópico. Esse tipo de implantação oferece armazenamento compartilhado virtual em um conjunto de VMs na parte superior de uma nuvem privada ou pública para que as soluções de alta disponibilidade do aplicativo podem ser usadas para aumentar a disponibilidade de aplicativos.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>Implantação em clusters de convidado da VM Iaas do Azure

[Os modelos do Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) foram publicado diminuir a complexidade, configurar melhor práticas e velocidade de suas implantações de espaços de armazenamento diretos em uma VM de Iaas do Azure. Isso é a solução recomendada para a implantação no Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>Requisitos

As seguintes considerações se aplicam ao implantar espaços de armazenamento diretos em um ambiente virtualizado.

> [!TIP]
> Os modelos do Azure configurará automaticamente o abaixo considerações para você e são a solução recomendada ao implantar em VMs IaaS do Azure.

-   Mínimo de 2 nós e máximo de 3 nós

-   implantações de nó 2 devem configurar uma testemunha (testemunha de nuvem ou testemunha de compartilhamento de arquivo)

-   implantações de 3 nós podem tolerar 1 nó para baixo e a perda de 1 ou mais discos em outro nó.  Se os 2 nós são desligadas, em seguida, os discos virtuais podemos estar offline até que um de nós de retorna.  

-   Configurar as máquinas virtuais a serem implantados em domínios de falha

    -   Azure – configurar conjunto de disponibilidade

    -   Hyper-V – configurar AntiAffinityClassNames nas VMs para separar as VMs entre os nós

    -   VMware – regra Antiafinidade de máquina virtual do VM de configurar, criando uma Rule DRS do tipo ' máquinas virtuais separadas "para separar as VMs em hosts ESX. Discos apresentados para uso com espaços de armazenamento diretos devem usar o adaptador de Paravirtual SCSI (PVSCSI). Para obter suporte PVSCSI com o Windows Server, consulte https://kb.vmware.com/s/article/1010398.

-   Aproveite a baixa latência / armazenamento de alto desempenho - armazenamento Premium do Azure gerenciado são necessários discos

-   Implantar um design de armazenamento simples sem nenhum dispositivo de armazenamento em cache configurado

-   Mínimo de 2 discos de dados virtual apresentados para cada máquina virtual (VHD / VHDX / VMDK)

    Esse número é diferente de implantações bare-metal, porque os discos virtuais podem ser implementados como arquivos que não são suscetíveis a falhas físicas.

-   Desabilite os recursos de substituição automática de unidade no serviço de integridade executando o seguinte cmdlet do PowerShell:

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   Sem suporte: Instantâneo do disco virtual de nível de host/restauração

    Em vez disso, use soluções de backup de nível de convidado tradicional para fazer backup e restaurar os dados nos volumes de espaços de armazenamento diretos.

-   Para fornecer maior resiliência a possíveis VHD / VHDX / latência de armazenamento VMDK em clusters de convidados, aumente o valor de tempo limite de e/s de espaços de armazenamento:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    O equivalente decimal 7530 Hexadecimal é 30000, que é de 30 segundos. Observe que o valor padrão é 1770 Hexadecimal ou 6000 Decimal, que é 6 segundos.

## <a name="see-also"></a>Consulte também

[Modelos de VM Iaas do Azure adicionais para a implantação de espaços de armazenamento diretos, vídeos e guias passo a passo](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/).

[Visão geral de direcionar a espaços de armazenamento adicionais] (https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
