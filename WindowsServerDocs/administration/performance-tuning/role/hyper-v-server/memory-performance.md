---
title: Desempenho de memória do Hyper-V
description: Considerações sobre memória no ajuste de desempenho do Hyper-V
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f0358747002eb850283c63770885d38872b7b903
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078262"
---
# <a name="hyper-v-memory-performance"></a>Desempenho de memória do Hyper-V


O hipervisor virtualiza a memória física convidada para isolar máquinas virtuais umas das outras e fornecer um espaço de memória contíguo baseado em zero para cada sistema operacional convidado, assim como em sistemas não virtualizados.

## <a name="correct-memory-sizing-for-child-partitions"></a>Corrigir o dimensionamento de memória para partições filho

Você deve dimensionar a memória da máquina virtual normalmente para aplicativos de servidor em um computador físico. Você deve dimensioná-lo para lidar razoavelmente com a carga esperada em horários normais e de pico, pois a memória insuficiente pode aumentar significativamente os tempos de resposta e O uso de CPU ou de e/s.

Você pode habilitar Memória Dinâmica para permitir que o Windows dimensione a memória da máquina virtual dinamicamente. Com Memória Dinâmica, se os aplicativos na experiência da máquina virtual estiverem fazendo grandes alocações de memória repentinas, você poderá aumentar o tamanho do arquivo de paginação para a máquina virtual para garantir o backup temporário enquanto Memória Dinâmica responde à pressão de memória.

Para obter mais informações sobre Memória Dinâmica, consulte [visão geral do Hyper-v memória dinâmica]( https://go.microsoft.com/fwlink/?linkid=834434) e [Guia de configuração do hyper-v memória dinâmica](https://go.microsoft.com/fwlink/?linkid=834435).

Ao executar o Windows na partição filho, você pode usar os seguintes contadores de desempenho em uma partição filho para identificar se a partição filho está passando por pressão de memória e provavelmente será melhor executada com um tamanho maior de memória de máquina virtual.

| Contador de desempenho                                                         | Valor de limite sugerido                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memória – bytes de reserva de cache em espera                                        | Soma de bytes de reserva de cache em espera e bytes de lista de páginas livres e zero devem ser 200 MB ou mais em sistemas com 1 GB e 300 MB ou mais em sistemas com 2 GB ou mais de RAM visível. |
| Memória – bytes de lista de páginas sem & livre                                        | Soma de bytes de reserva de cache em espera e bytes de lista de páginas livres e zero devem ser 200 MB ou mais em sistemas com 1 GB e 300 MB ou mais em sistemas com 2 GB ou mais de RAM visível. |
| Memória – entrada de páginas/s                                                    | A média em um período de 1 hora é menor que 10.                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>Corrigir o dimensionamento de memória para a partição raiz

A partição raiz deve ter memória suficiente para fornecer serviços como virtualização de e/s, instantâneo de máquina virtual e gerenciamento para dar suporte às partições filho.

O Hyper-V no Windows Server 2016 monitora a integridade do tempo de execução do sistema operacional de gerenciamento da partição raiz para determinar a quantidade de memória que pode ser alocada com segurança para partições filho, garantindo, ao mesmo tempo, alto desempenho e confiabilidade da partição raiz.

## <a name="additional-references"></a>Referências adicionais

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura Hyper-V](architecture.md)

-   [Configuração do servidor do Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de E/S de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de E/S de rede do Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
