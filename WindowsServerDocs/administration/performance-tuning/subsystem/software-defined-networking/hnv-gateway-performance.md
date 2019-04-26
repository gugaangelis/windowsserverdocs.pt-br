---
title: Desempenho do Gateway de HNV no Software de ajuste definida redes
description: Diretrizes de redes definidas por Software de ajuste de desempenho de gateway HNV
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 217428b84a00b2e2231a15cb3878d0abcec1d9ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827317"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>Desempenho do Gateway de HNV no Software de ajuste definida redes

Este tópico fornece especificações de hardware e recomendações de configuração para servidores que estão executando o Hyper-V e hospedando máquinas virtuais de Gateway do Windows Server, além dos parâmetros de configuração para máquinas virtuais de Gateway do Windows Server (VMs) . Para extrair melhor desempenho de VMs de gateway do Windows Server, espera-se que essas diretrizes são seguidas.
As seções a seguir contêm requisitos de hardware e configuração quando você implementa o Gateway do Windows Server.
1. Recomendações de hardware do Hyper-V
2. Configuração do host Hyper-V
3. Configuração de VM do gateway do Windows Server

## <a name="hyper-v-hardware-recommendations"></a>Recomendações de hardware do Hyper-V

A seguir está a configuração de hardware mínimos recomendados para cada servidor que está executando o Windows Server 2016 e do Hyper-V.

| Componente de servidor               | Especificação                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Unidade de processamento central (CPU)  | Nós de arquitetura de memória não uniforme (NUMA): 2 <br> Se houver vários gateway do Windows Server VMs no host, para melhor desempenho, cada VM de gateway deve ter acesso completo a um nó NUMA. E ele deve ser diferente do nó NUMA usado pelo adaptador físico do host. |
| Núcleos por nó NUMA            | 2                                                                                                                                                                                                                                                                               |
| Hyper-Threading                | Desabilitado. O Hyper-Threading não melhora o desempenho do Gateway do Windows Server.                                                                                                                                                                                           |
| Memória RAM     | 48 GB                                                                                                                                                                                                                                                                           |
| Placas de interface de rede (NICs) | Duas NICs de 10 GB, o desempenho do gateway dependem de taxa de linha. Se a taxa de linha for menor que 10 Gbps, os números de taxa de transferência de túnel gateway também ficará inativo pelo mesmo fator.                                                                                          |

Verifique se o número de processadores virtuais que são atribuídos a uma VM de Gateway do Windows Server não excede o número de processadores no nó NUMA. Por exemplo, se um nó NUMA tiver 8 núcleos, o número de processadores virtuais deverá ser menor ou igual a 8. Para obter melhor desempenho, ele deve ser 8. Para encontrar o número de nós NUMA e o número de núcleos por nó NUMA, execute o seguinte script do Windows PowerShell em cada host Hyper-V:

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

Uma VM do gateway com oito processadores virtuais e pelo menos 8GB de RAM é recomendado ao selecionar o número de VMs de gateway para instalar em cada host Hyper-V quando cada nó NUMA tem oito núcleos. Nesse caso, um nó NUMA é dedicado para a máquina host.

## <a name="hyper-v-host-configuration"></a>Configuração do Host do Hyper-V

A seguir está a configuração recomendada para cada servidor que está executando o Windows Server 2016 e do Hyper-V e cuja carga de trabalho é executar VMs de Gateway do Windows Server. Essas instruções de configuração incluem o uso de exemplos de comando do Windows PowerShell. Esses exemplos contêm espaços reservados para valores reais que você precisa fornecer quando executa os comandos em seu ambiente. Por exemplo, os espaços reservados de nome de adaptador de rede são "NIC1? e "NIC2.? Quando você executar comandos que usem esses espaços reservados, utilize os nomes reais dos adaptadores de rede em seus servidores, em vez de usar os espaços reservados, ou os comandos falharão.

>[!Note]
> Para executar os seguintes comandos do Windows PowerShell, você deve ser um membro do grupo Administradores.

| Item de configuração                          | Configuração do Windows Powershell                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Agrupamento incorporado de comutador                     | Quando você cria um vswitch com vários adaptadores de rede, ele automaticamente habilitado switch embedded agrupamento para esses adaptadores. <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> Não há suporte para agrupamento tradicional por meio de LBFO com SDN no Windows Server 2016. Agrupamento incorporado do comutador permite que você use o mesmo conjunto de NICs para o tráfego virtual e o tráfego RDMA. Isso não era compatível com o agrupamento NIC com base em LBFO.                                                        |
| Moderação da Interrupção em NICs físicas       | Use as configurações padrão. Para verificar a configuração, você pode usar o seguinte comando do Windows PowerShell: ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| Tamanho dos Buffers de Recepção nas NICs físicas       | Você pode verificar se as NICs físicas suportam a configuração desse parâmetro, executando o comando ```Get-NetAdapterAdvancedProperty```. Se eles não dão suporte a esse parâmetro, a saída do comando não inclui a propriedade "Buffers de recepção.? Se as NICs não tiverem suporte para esse parâmetro, você poderá usar o seguinte comando do Windows PowerShell para definir o tamanho dos Buffers de Recepção: <br>```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Receive Buffers�? –DisplayValue 3000``` <br>                          |
| Tamanho dos Buffers de Envio nas NICs físicas          | Você pode verificar se as NICs físicas suportam a configuração desse parâmetro, executando o comando ```Get-NetAdapterAdvancedProperty```. Se as NICs não oferecer suporte a esse parâmetro, a saída do comando não inclui a propriedade "Buffers de envio.? Se as NICs não tiverem suporte para esse parâmetro, você pode usar o seguinte comando do Windows PowerShell para definir o tamanho dos Buffers de Recepção: <br> ```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Transmit Buffers�? –DisplayValue 3000``` <br>                           |
| RSS (Receive Side Scaling) nas NICs físicas | Você pode verificar se suas NICs físicas têm o RSS habilitado executando o comando Get-NetAdapterRss do Windows PowerShell. Você pode usar os seguintes comandos do Windows PowerShell para habilitar e configurar o RSS em seus adaptadores de rede: <br> ```Enable-NetAdapterRss “NIC1�?,�?NIC2�?```<br> ```Set-NetAdapterRss “NIC1�?,�?NIC2�? –NumberOfReceiveQueues 16 -MaxProcessors``` <br> OBSERVAÇÃO: Se VMMQ ou VMQ estiver habilitado, RSS não precisa ser habilitado nos adaptadores de rede física. Você pode habilitá-lo nos adaptadores de rede virtual do host |
| VMMQ                                        | Para habilitar VMMQ para uma VM, execute o seguinte comando: <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> OBSERVAÇÃO: Nem todos os adaptadores de rede oferecem suporte a VMMQ. Atualmente, ele é compatível com a série de 45xxx Chelsio T5 e T6, CX Mellanox-3 e 4 do CX e QLogic                                                                                                                                                                                                                                      |
| VMQ (Fila da Máquina Virtual) na equipe da NIC | Você pode habilitar VMQ em sua equipe de conjunto, usando o seguinte comando do Windows PowerShell: <br>```Enable-NetAdapterVmq``` <br> OBSERVAÇÃO: Isso deve ser habilitado somente se o hardware não oferece suporte a VMMQ. Se houver suporte, VMMQ deve ser habilitada para melhorar o desempenho.                                                                                                                                                                                                                                                               |
>[!Note]
> VMQ e vRSS entram em imagem apenas quando a carga na máquina virtual for alta e a CPU está sendo utilizada ao máximo. Só então será pelo menos um processador núcleo max-out. VMQ e vRSS, em seguida, será útil para ajudar a distribuir a carga de processamento entre vários núcleos. Isso não é aplicável para o tráfego de IPsec, pois o tráfego IPsec está restrita a um único núcleo.

## <a name="windows-server-gateway-vm-configuration"></a>Configuração da VM de Gateway do Windows Server

Em ambos os hosts do Hyper-V, você pode configurar várias VMs que são configuradas como gateways de Gateway do Windows Server. Você pode usar o Gerenciador de Comutador Virtual para criar um Comutador Virtual Hyper-V que está associado à equipe da NIC no host Hyper-V. Observe que, para melhor desempenho, você deve implantar um único gateway de VM em um host Hyper-V.
A seguir, está a configuração recomendada para cada VM de Gateway do Windows Server.

| Item de configuração                 | Configuração do Windows Powershell                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memória                             | 8 GB                                                                                                                                                                                                                                                                                                                                                                                           |
| Número de adaptadores de rede virtual | 3 NICs com itens específicos a seguir usa: 1 para gerenciamento, que é usado pelo sistema operacional de gerenciamento, 1 externo que fornece acesso a redes externas, 1 interno que fornece acesso apenas às redes internas.                                                                                                                                                            |
| Receive Side Scaling (RSS)         | Você pode manter o RSS configurações padrão para a NIC de gerenciamento. O seguinte exemplo de configuração é para uma VM que tem 8 processadores virtuais. Para as NICs externas e internas, você pode habilitar o RSS com BaseProcNumber definido como 0 e MaxRssProcessors definido como 8 usando o seguinte comando do Windows PowerShell: <br> ```Set-NetAdapterRss “Internal�?,�?External�? –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| Buffer lateral de envio                   | Você pode manter o Buffer lateral de envio configurações padrão para a NIC de gerenciamento. Para as NICs internas e externas, você pode configurar o Buffer lateral de envio com 32 MB de RAM usando o seguinte comando do Windows PowerShell: <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Send Buffer Size�? –DisplayValue “32MB�?``` <br>                                                       |
| Buffer lateral de recepção                | Você pode manter o Buffer lateral de recepção configurações padrão para a NIC de gerenciamento. Para as NICs internas e externas, você pode configurar o Buffer lateral de recepção com 16 MB de RAM usando o seguinte comando do Windows PowerShell: <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Receive Buffer Size�? –DisplayValue “16MB�?``` <br>                                            |
| Otimização de Encaminhamento               | Você pode manter o padrão as configurações de otimização de encaminhamento para a NIC de gerenciamento. Para as NICs internas e externas, você pode habilitar a otimização de encaminhamento usando o seguinte comando do Windows PowerShell: <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Forward Optimization�? –DisplayValue “1�?``` <br>                                                                      |
