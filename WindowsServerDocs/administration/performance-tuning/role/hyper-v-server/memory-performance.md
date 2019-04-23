---
title: Desempenho de memória do Hyper-V
description: Considerações de memória no Hyper-V de ajuste de desempenho
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 63a1b654b8ac52725cc5dd87c8b245f9dfaf40f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848067"
---
# <a name="hyper-v-memory-performance"></a>Desempenho de memória do Hyper-V


O hipervisor virtualiza a memória física do convidado para isolar as máquinas virtuais entre si e fornecer um espaço de memória contígua, com base em zero para cada sistema operacional convidado, como em sistemas não-virtualizados.

## <a name="correct-memory-sizing-for-child-partitions"></a>Dimensionamento correto de memória para partições filho

Você deve dimensionar a memória da máquina virtual como faria normalmente para aplicativos de servidor em um computador físico. Você deve dimensioná-lo para trabalhar razoavelmente com a carga esperada em comum e momentos de pico porque memória insuficiente pode aumentar significativamente os tempos de resposta e utilização de CPU ou e/s.

Você pode habilitar a memória dinâmica permitir que o Windows para a memória da máquina virtual de tamanho dinamicamente. Com a memória dinâmica, se os aplicativos na máquina virtual tiver problemas fazendo alocações de memória repentinos grandes, você pode aumentar o tamanho do arquivo de paginação para a máquina virtual garantir que o backup temporário enquanto a memória dinâmica responde à pressão de memória.

Para obter mais informações sobre a memória dinâmica, consulte [Hyper-V Dynamic Memory Overview]( https://go.microsoft.com/fwlink/?linkid=834434) e [guia de configuração de memória dinâmica do Hyper-V](https://go.microsoft.com/fwlink/?linkid=834435).

Ao executar o Windows na partição filho, você pode usar os seguintes contadores de desempenho dentro de uma partição filho para identificar se a partição filho está passando por pressão de memória e é provável que funcionar melhor com um tamanho maior de memória de máquina virtual.

| Contador de desempenho                                                         | Valor de limite sugerido                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memória-Bytes de reserva de Cache em espera                                        | Soma dos Bytes de reserva de Cache em espera e gratuito e Zero Bytes de lista de página deve ser 200 MB ou mais em sistemas com 1 GB e 300 MB ou mais em sistemas com 2 GB ou mais de RAM visível. |
| Zero Bytes de lista de páginas e de memória – gratuita                                        | Soma dos Bytes de reserva de Cache em espera e gratuito e Zero Bytes de lista de página deve ser 200 MB ou mais em sistemas com 1 GB e 300 MB ou mais em sistemas com 2 GB ou mais de RAM visível. |
| Memória – entrada de páginas/s                                                    | Média durante um período de 1 hora é menor que 10.                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>Dimensionamento correto de memória para a partição raiz

A partição raiz deve ter memória suficiente para fornecer serviços como a virtualização de e/s, instantâneo de máquina virtual e gerenciamento para oferecer suporte a partições filho.

Hyper-V no Windows Server 2016 monitora a integridade de tempo de execução do sistema de operacional de gerenciamento da partição raiz para determinar a quantidade de memória pode ser alocada com segurança a partições filho, e ainda garantir alto desempenho e confiabilidade da partição raiz.

## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura do Hyper-V](architecture.md)

-   [Configuração de servidor Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de e/s de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de e/s de rede de Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
