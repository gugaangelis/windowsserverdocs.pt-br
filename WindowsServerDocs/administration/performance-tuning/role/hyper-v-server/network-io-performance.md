---
title: Desempenho de e/s de rede do Hyper-V
description: Considerações de desempenho de e/s de rede no ajuste de desempenho do Hyper-V
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 10a2c21dc581864796ef2a301033377da09a5f7f
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078243"
---
# <a name="hyper-v-network-io-performance"></a>Desempenho de e/s de rede do Hyper-V

O servidor 2016 contém vários aprimoramentos e novas funcionalidades para otimizar o desempenho da rede no Hyper-V.  A documentação sobre como otimizar o desempenho da rede será incluída em uma versão futura deste artigo.

## <a name="live-migration"></a>Migração dinâmica

Migração ao Vivo permite mover de forma transparente as máquinas virtuais em execução de um nó de um cluster de failover para outro nó no mesmo cluster sem uma conexão de rede descartada ou tempo de inatividade percebido.

> [!NOTE]
> O clustering de failover requer armazenamento compartilhado para os nós de cluster.

O processo de mover uma máquina virtual em execução pode ser dividido em duas fases principais. A primeira fase copia a memória da máquina virtual do host atual para o novo host. A segunda fase transfere o estado da máquina virtual do host atual para o novo host. As durações de ambas as fases são muito determinadas pela velocidade na qual os dados podem ser transferidos do host atual para o novo host.

Fornecer uma rede dedicada para o tráfego de migração ao vivo ajuda a minimizar o tempo necessário para concluir uma migração ao vivo e garante tempos de migração consistentes.

![exemplo de configuração de migração dinâmica do Hyper-v](../../media/perftune-guide-live-migration.png)

Além disso, aumentar o número de buffers de envio e recebimento em cada adaptador de rede que está envolvido na migração pode melhorar o desempenho da migração.

O Windows Server 2012 R2 introduziu uma opção para acelerar a Migração ao Vivo compactando a memória antes de transferi-la pela rede ou usar o RDMA (acesso remoto direto à memória), se o hardware oferecer suporte a ela.

## <a name="additional-references"></a>Referências adicionais

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura Hyper-V](architecture.md)

-   [Configuração do servidor do Hyper-V](configuration.md)

-   [Desempenho do processador do Hyper-V](processor-performance.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de E/S de armazenamento do Hyper-V](storage-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
