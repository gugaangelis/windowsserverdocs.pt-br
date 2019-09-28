---
title: Ajuste de desempenho de gateway HNV em redes definidas por software
description: Diretrizes de ajuste de desempenho do gateway HNV em redes definidas por software
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 907b160b143af18a8ede3a9a7975fa8b22753118
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383499"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>Ajuste de desempenho de gateway HNV em redes definidas por software

Este tópico fornece especificações de hardware e recomendações de configuração para servidores que executam o Hyper-V e hospedando máquinas virtuais do gateway do Windows Server, além de parâmetros de configuração para VMs (máquinas virtuais) do gateway do Windows Server . Para extrair o melhor desempenho de VMs do gateway do Windows Server, espera-se que essas diretrizes sejam seguidas.
As seções a seguir contêm requisitos de hardware e configuração quando você implementa o Gateway do Windows Server.
1. Recomendações de hardware do Hyper-V
2. Configuração do host Hyper-V
3. Configuração da VM do gateway do Windows Server

## <a name="hyper-v-hardware-recommendations"></a>Recomendações de hardware do Hyper-V

A seguir está a configuração mínima de hardware recomendada para cada servidor que está executando o Windows Server 2016 e o Hyper-V.

| Componente de servidor               | Especificação                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Unidade de processamento central (CPU)  | Nós NUMA (arquitetura de memória não uniforme): 2 <br> Se houver várias VMs de gateway do Windows Server no host, para melhor desempenho, cada VM de gateway deverá ter acesso completo a um nó NUMA. E deve ser diferente do nó NUMA usado pelo adaptador físico do host. |
| Núcleos por nó NUMA            | 2                                                                                                                                                                                                                                                                               |
| Hyperthreading                | Desabilitado. O Hyper-Threading não melhora o desempenho do Gateway do Windows Server.                                                                                                                                                                                           |
| Memória RAM     | 48 GB                                                                                                                                                                                                                                                                           |
| Placas de interface de rede (NICs) | NICs de 2 10 GB, o desempenho do gateway dependerá da taxa de linha. Se a taxa de linha for menor que 10 Gbps, os números de taxa de transferência do túnel do gateway também ficarão inativos pelo mesmo fator.                                                                                          |

Verifique se o número de processadores virtuais que são atribuídos a uma VM de Gateway do Windows Server não excede o número de processadores no nó NUMA. Por exemplo, se um nó NUMA tiver 8 núcleos, o número de processadores virtuais deverá ser menor ou igual a 8. Para obter o melhor desempenho, deve ser 8. Para encontrar o número de nós NUMA e o número de núcleos por nó NUMA, execute o seguinte script do Windows PowerShell em cada host Hyper-V:

```PowerShell
$nodes = [object[]] $(gwmi –Namespace root\virtualization\v2 -Class MSVM_NumaNode)
$cores = ($nodes | Measure-Object NumberOfProcessorCores -sum).Sum
$lps = ($nodes | Measure-Object NumberOfLogicalProcessors -sum).Sum


Write-Host "Number of NUMA Nodes: ", $nodes.count
Write-Host ("Total Number of Cores: ", $cores)
Write-Host ("Total Number of Logical Processors: ", $lps)
```

>[!Important]
> A alocação de processadores virtuais em nós NUMA pode ter um impacto negativo no desempenho no Gateway do Windows Server. A execução de várias VMs, cada uma com processadores virtuais de um nó NUMA, provavelmente proporciona melhor desempenho agregado do que uma única VM à qual todos os processadores virtuais são atribuídos.

Uma VM de gateway com oito processadores virtuais e pelo menos 8 GB de RAM é recomendada ao selecionar o número de VMs de gateway a serem instaladas em cada host Hyper-V quando cada nó NUMA tem oito núcleos. Nesse caso, um nó NUMA é dedicado ao computador host.

## <a name="hyper-v-host-configuration"></a>Configuração de host do Hyper-V

Veja a seguir a configuração recomendada para cada servidor que está executando o Windows Server 2016 e o Hyper-V e cuja carga de trabalho é executar VMs do gateway do Windows Server. Essas instruções de configuração incluem o uso de exemplos de comando do Windows PowerShell. Esses exemplos contêm espaços reservados para valores reais que você precisa fornecer quando executa os comandos em seu ambiente. Por exemplo, os espaços reservados do nome do adaptador de rede são "NIC1" e "NIC2". Quando você executar comandos que usem esses espaços reservados, utilize os nomes reais dos adaptadores de rede em seus servidores, em vez de usar os espaços reservados, ou os comandos falharão.

>[!Note]
> Para executar os seguintes comandos do Windows PowerShell, você deve ser um membro do grupo Administradores.

| Item de configuração                          | Configuração do Windows PowerShell                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Agrupamento incorporado de comutador                     | Quando você cria um vSwitch com vários adaptadores de rede, ele automaticamente habilitou o agrupamento integrado de comutadores para esses adaptadores. <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> O agrupamento tradicional por meio do LBFO não tem suporte com o SDN no Windows Server 2016. Alternar o agrupamento incorporado permite que você use o mesmo conjunto de NICs para o tráfego virtual e o tráfego de RDMA. Não há suporte para isso com o agrupamento NIC baseado em LBFO.                                                        |
| Moderação da Interrupção em NICs físicas       | Use as configurações padrão. Para verificar a configuração, você pode usar o seguinte comando do Windows PowerShell: ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| Tamanho dos Buffers de Recepção nas NICs físicas       | Você pode verificar se as NICs físicas dão suporte à configuração desse parâmetro executando o comando ```Get-NetAdapterAdvancedProperty```. Se eles não oferecerem suporte a esse parâmetro, a saída do comando não incluirá a propriedade "buffers de recebimento". Se as NICs não tiverem suporte para esse parâmetro, você poderá usar o seguinte comando do Windows PowerShell para definir o tamanho dos Buffers de Recepção: <br>```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Receive Buffers" –DisplayValue 3000``` <br>                          |
| Tamanho dos Buffers de Envio nas NICs físicas          | Você pode verificar se as NICs físicas dão suporte à configuração desse parâmetro executando o comando ```Get-NetAdapterAdvancedProperty```. Se as NICs não suportarem esse parâmetro, a saída do comando não incluirá a propriedade "buffers de envio". Se as NICs não tiverem suporte para esse parâmetro, você pode usar o seguinte comando do Windows PowerShell para definir o tamanho dos Buffers de Recepção: <br> ```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Transmit Buffers" –DisplayValue 3000``` <br>                           |
| RSS (Receive Side Scaling) nas NICs físicas | Você pode verificar se as NICs físicas têm o RSS habilitado executando o comando Get-NetAdapterRss do Windows PowerShell. Você pode usar os seguintes comandos do Windows PowerShell para habilitar e configurar o RSS em seus adaptadores de rede: <br> ```Enable-NetAdapterRss "NIC1","NIC2"```<br> ```Set-NetAdapterRss "NIC1","NIC2" –NumberOfReceiveQueues 16 -MaxProcessors``` <br> OBSERVAÇÃO:  Se VMMQ ou VMQ estiver habilitado, o RSS não precisará ser habilitado nos adaptadores de rede física. Você pode habilitá-lo nos adaptadores de rede virtual do host |
| VMMQ                                        | Para habilitar o VMMQ para uma VM, execute o seguinte comando: <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> OBSERVAÇÃO:  Nem todos os adaptadores de rede dão suporte a VMMQ. Atualmente, há suporte para o Chelsio T5 e o T6, o Mellanox CX-3 e o CX-4 e o QLogic 45xxx Series                                                                                                                                                                                                                                      |
| VMQ (Fila da Máquina Virtual) na equipe da NIC | Você pode habilitar a VMQ em sua equipe de conjunto usando o seguinte comando do Windows PowerShell: <br>```Enable-NetAdapterVmq``` <br> OBSERVAÇÃO:  Isso deve ser habilitado somente se o HW não oferecer suporte a VMMQ. Se houver suporte, VMMQ deverá ser habilitado para melhorar o desempenho.                                                                                                                                                                                                                                                               |
>[!Note]
> A VMQ e a vRSS entram em imagem somente quando a carga na VM é alta e a CPU está sendo utilizada para o máximo. Em seguida, haverá, pelo menos, um núcleo do processador Max out. A VMQ e o vRSS serão benéficos para ajudar a distribuir a carga de processamento entre vários núcleos. Isso não é aplicável ao tráfego IPsec, pois o tráfego IPsec é restrito a um único núcleo.

## <a name="windows-server-gateway-vm-configuration"></a>Configuração da VM de Gateway do Windows Server

Em ambos os hosts do Hyper-V, você pode configurar várias VMs configuradas como gateways com o gateway do Windows Server. Você pode usar o Gerenciador de Comutador Virtual para criar um Comutador Virtual Hyper-V que está associado à equipe da NIC no host Hyper-V. Observe que para obter o melhor desempenho, você deve implantar uma única VM de gateway em um host Hyper-V.
A seguir, está a configuração recomendada para cada VM de Gateway do Windows Server.

| Item de configuração                 | Configuração do Windows PowerShell                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memória                             | 8 GB                                                                                                                                                                                                                                                                                                                                                                                           |
| Número de adaptadores de rede virtual | 3 NICs com os seguintes usos específicos: 1 para o gerenciamento usado pelo sistema operacional de gerenciamento, 1 externo que fornece acesso a redes externas, 1 que é interno que fornece acesso somente a redes internas.                                                                                                                                                            |
| Receive Side Scaling (RSS)         | Você pode manter as configurações de RSS padrão para a NIC de gerenciamento. O seguinte exemplo de configuração é para uma VM que tem 8 processadores virtuais. Para as NICs externas e internas, você pode habilitar o RSS com BaseProcNumber definido como 0 e MaxRssProcessors definido como 8 usando o seguinte comando do Windows PowerShell: <br> ```Set-NetAdapterRss "Internal","External" –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| Buffer do lado de envio                   | Você pode manter as configurações de buffer do lado de envio padrão para a NIC de gerenciamento. Para as NICs internas e externas, você pode configurar o buffer do lado de envio com 32 MB de RAM usando o seguinte comando do Windows PowerShell: <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Send Buffer Size" –DisplayValue "32MB"``` <br>                                                       |
| Buffer do lado de recebimento                | Você pode manter as configurações de buffer do lado de recebimento padrão para a NIC de gerenciamento. Para as NICs internas e externas, você pode configurar o buffer do lado de recebimento com 16 MB de RAM usando o seguinte comando do Windows PowerShell: <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Receive Buffer Size" –DisplayValue "16MB"``` <br>                                            |
| Otimização de Encaminhamento               | Você pode manter as configurações de otimização de encaminhamento padrão para a NIC de gerenciamento. Para as NICs internas e externas, você pode habilitar a otimização de encaminhamento usando o seguinte comando do Windows PowerShell: <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Forward Optimization" –DisplayValue "1"``` <br>                                                                      |
