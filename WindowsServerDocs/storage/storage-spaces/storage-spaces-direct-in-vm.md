---
title: Usando espaços de armazenamento diretos em uma máquina virtual
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Como implantar espaços de armazenamento diretos em um cluster de convidado de máquina virtual - por exemplo, no Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: a741475d09ab7f7ab29f158ef55378ca9a37beac
ms.sourcegitcommit: 52883945fd6e6bb5c24bd81944ca4bc0c5e6a216
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2018
ms.locfileid: "8964608"
---
# Usando espaços de armazenamento diretos em clusters de máquina virtual convidada

> Aplica-se a: Windows Server 2019, Windows Server 2016

Você pode implantar espaços de armazenamento diretos em um cluster de servidores físicos ou em clusters de convidado de máquina virtual conforme discutido neste tópico. Esse tipo de implantação fornece armazenamento compartilhado virtual em um conjunto de máquinas virtuais em cima de uma nuvem privada ou pública para que as soluções de alta disponibilidade do aplicativo podem ser usadas para aumentar a disponibilidade de aplicativos.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## Implantar em clusters de convidado VM Iaas do Azure

[Modelos Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) foram publicado diminuir a complexidade, configurar melhor práticas e velocidade de suas implantações de espaços de armazenamento diretos em uma VM Iaas do Azure. Essa é a solução recomendada para a implantação no Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## Requisitos

As considerações a seguir se aplicam ao implantar espaços de armazenamento diretos em um ambiente virtualizado.

> [!TIP]
> Modelos Azure configurará automaticamente o abaixo considerações por você e são a solução recomendada ao implantar em máquinas virtuais IaaS do Azure.

-   2 nós mínimo e máximo de 3 nós

-   implantações de 2 nós devem configurar uma testemunha (testemunha de compartilhamento de arquivos ou testemunha de nuvem)

-   implantações de 3 nós podem tolerar 1 nó para baixo e a perda de 1 ou mais discos em outro nó.  Se 2 nós são desligamento, os discos virtuais podemos ser offline até que um de nós retorna.  

-   Configure as máquinas virtuais para ser implantado em domínios de falha

    -   Azure – configurar o conjunto de disponibilidade

    -   Hyper-V – configurar AntiAffinityClassNames nas VMs para separar as VMs em todos os nós

    -   VMware – regra configurar VM-VM antiafinidade criando uma Rule DRS do tipo ' máquinas virtuais separadas "para separar as VMs em hosts ESX. Discos apresentados para uso com espaços de armazenamento diretos devem usar o adaptador de SCSI Paravirtual (PVSCSI). Para obter suporte PVSCSI com o Windows Server, consulte https://kb.vmware.com/s/article/1010398.

-   Aproveite a baixa latência / armazenamento de alto desempenho - armazenamento Premium do Azure gerenciado discos são necessários

-   Implantar um projeto de armazenamento simples com dispositivos de cache não configurados

-   Mínimo de 2 discos de dados virtual apresentados para cada VM (VHD / VHDX / VMDK)

    Esse número é diferente de implantações bare-metal porque os discos virtuais podem ser implementados como arquivos que não são suscetíveis a falhas físicas.

-   Desative os recursos de substituição de unidade automática no serviço de integridade, executando o seguinte cmdlet do PowerShell:

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   Não tem suporte: hospedar instantâneo/restaurar o nível de disco virtual

    Em vez disso, use soluções de backup de nível de convidado tradicional para fazer backup e restaurar os dados em volumes de espaços de armazenamento diretos.

-   Para dar maior resiliência para possível VHD / VHDX / latência de armazenamento VMDK em clusters de convidado, aumente o valor de tempo limite de e/s de espaços de armazenamento:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    O equivalente decimal 7530 Hexadecimal é 30000, que é 30 segundos. Observe que o valor padrão é 1770 Hexadecimal ou 6000 Decimal, que é 6 segundos.

## Consulte também

[Modelos de VM Iaas do Azure adicional para espaços de armazenamento implantando diretos, vídeos e guias passo a passo](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/).

[Espaços de armazenamento adicionais diretos visão geral] (https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
