---
title: Desempenho de e/s de rede do Hyper-V
description: Considerações de desempenho de e/s de rede no ajuste de desempenho do Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e8f4261c11a63786c2d170105fb0fa65dc6966a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385125"
---
# <a name="hyper-v-network-io-performance"></a>Desempenho de e/s de rede do Hyper-V

O servidor 2016 contém vários aprimoramentos e novas funcionalidades para otimizar o desempenho da rede no Hyper-V.  A documentação sobre como otimizar o desempenho da rede será incluída em uma versão futura deste artigo.

## <a name="live-migration"></a>Migração ao vivo

Migração ao Vivo permite mover de forma transparente as máquinas virtuais em execução de um nó de um cluster de failover para outro nó no mesmo cluster sem uma conexão de rede descartada ou tempo de inatividade percebido.

> [!NOTE]
> O clustering de failover requer armazenamento compartilhado para os nós de cluster.

O processo de mover uma máquina virtual em execução pode ser dividido em duas fases principais. A primeira fase copia a memória da máquina virtual do host atual para o novo host. A segunda fase transfere o estado da máquina virtual do host atual para o novo host. As durações de ambas as fases são muito determinadas pela velocidade na qual os dados podem ser transferidos do host atual para o novo host.

Fornecer uma rede dedicada para o tráfego de migração ao vivo ajuda a minimizar o tempo necessário para concluir uma migração ao vivo e garante tempos de migração consistentes.

![exemplo de configuração de migração dinâmica do Hyper-v](../../media/perftune-guide-live-migration.png)

Além disso, aumentar o número de buffers de envio e recebimento em cada adaptador de rede que está envolvido na migração pode melhorar o desempenho da migração.

O Windows Server 2012 R2 introduziu uma opção para acelerar a Migração ao Vivo compactando a memória antes de transferi-la pela rede ou usar o RDMA (acesso remoto direto à memória), se o hardware oferecer suporte a ela.

## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura Hyper-V](architecture.md)

-   [Configuração do servidor do Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de E/S de armazenamento do Hyper-V](storage-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
