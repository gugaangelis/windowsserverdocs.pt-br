---
title: Desempenho de rede do Hyper-V e/s
description: Considerações de desempenho de e/s no ajuste de desempenho do Hyper-V de rede
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 6dd9d03cce71c42e2ecc1388519de049a5e4e8dd
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266720"
---
# <a name="hyper-v-network-io-performance"></a>Desempenho de rede do Hyper-V e/s

Server 2016 contém vários aprimoramentos e novas funcionalidades para otimizar o desempenho de rede no Hyper-V.  Documentação sobre como otimizar o desempenho de rede será incluída em uma versão futura deste artigo.

## <a name="live-migration"></a>Migração ao vivo

Migração ao vivo permite mover de maneira transparente máquinas virtuais em execução de um nó de um cluster de failover para outro nó no mesmo cluster sem uma conexão de rede perdida ou tempo de inatividade perceptível.

> [!Note]
> Clustering de failover exige armazenamento compartilhado para os nós de cluster.

O processo de mover uma máquina virtual em execução pode ser dividido em duas fases principais. A primeira fase copia a memória da máquina virtual do host atual para o novo host. A segunda fase transfere o estado da máquina virtual do host atual para o novo host. As durações de ambas as fases bastante é determinada pela velocidade na qual os dados podem ser transferidos do host atual para o novo host.

Fornecendo uma rede dedicada para migração ao vivo tráfego ajuda a minimizar o tempo necessário para concluir uma migração ao vivo e garante que os tempos de migração consistente.

![exemplo de configuração de migração ao vivo do hyper-v](../../media/perftune-guide-live-migration.png)

Além disso, aumentar o número de enviar e receber buffers em cada rede adaptador que está envolvido na migração pode melhorar o desempenho de migração.

Windows Server 2012 R2 introduziu uma opção para acelerar a migração ao vivo ao compactar memória antes de serem transferidos pela rede ou usar direto memória RDMA (acesso remoto), se seu hardware dá suporte a ele.

## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura Hyper-V](architecture.md)

-   [Configuração do servidor do Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de E/S de armazenamento do Hyper-V](storage-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
