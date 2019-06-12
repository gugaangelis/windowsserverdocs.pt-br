---
title: Desempenho do processador do Hyper-V
description: Considerações de desempenho do processador no ajuste de desempenho do Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f16ee9cff9c244a8c579e008bced1e90b1a20673
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435595"
---
# <a name="hyper-v-processor-performance"></a>Desempenho do processador do Hyper-V


## <a name="virtual-machine-integration-services"></a>Serviços de integração de máquina virtual

Os serviços de integração de máquina Virtual incluem os drivers habilitados para os dispositivos de e/s de específico do Hyper-V, o que reduz significativamente CPU sobrecarga de e/s em comparação com os dispositivos emulados. Você deve instalar a versão mais recente dos serviços de integração de máquina Virtual em cada máquina virtual com suporte. A diminuição de serviços convidados o uso da CPU dos convidados, do estado ocioso para intensamente usado convidados e melhora a taxa de transferência de e/s. Essa é a primeira etapa no ajuste de desempenho em um servidor que executa o Hyper-V. Para obter uma lista de sistemas operacionais convidados com suporte, consulte [visão geral do Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="virtual-processors"></a>Processadores virtuais

Hyper-V no Windows Server 2016 dá suporte a um máximo de 240 processadores virtuais por máquina virtual. Máquinas virtuais com cargas que não estão com uso intensivo de CPU deve ser configuradas para usar um processador virtual. Isso é devido à sobrecarga adicional que está associada com vários processadores virtuais, como os custos de sincronização adicional no sistema operacional convidado.

Se a máquina virtual exigir mais de uma CPU de processamento sob carga de pico, aumente o número de processadores virtuais.

## <a name="background-activity"></a>Atividade em segundo plano

Minimizando a atividade em segundo plano em máquinas virtuais ociosas libera ciclos de CPU que podem ser usados em outros lugares por outras máquinas virtuais. Convidados do Windows normalmente usam menos de 1% de uma CPU quando estão ociosos. A seguir estão várias práticas recomendadas para minimizar o uso da CPU de uma máquina virtual do plano de fundo:

-   Instale a versão mais recente dos serviços de integração de máquina Virtual.

-   Remova o adaptador de rede emulado por meio da caixa de diálogo de configurações de máquina virtual (adaptador de usar o Hyper-V-específico da Microsoft).

-   Remover dispositivos não utilizados, como o CD-ROM e COM a porta ou desconecte a mídia.

-   Manter o sistema de operacional de convidado do Windows na tela de logon quando não está sendo usado e desabilite a proteção de tela.

-   Examine as tarefas agendadas e serviços que são habilitados por padrão.

-   Examine os provedores de rastreamento ETW que são ativados por padrão, executando **logman.exe consultar - ets**

-   Melhore os aplicativos de servidor para reduzir a atividade periódica (por exemplo, temporizadores).

-   Feche o Gerenciador de servidores em sistemas operacionais de host e convidado.

-   Não deixe o Gerenciador do Hyper-V em execução, pois ele atualiza constantemente a miniatura da máquina virtual.

A seguir estão as práticas recomendadas adicionais para configurar uma *versão do cliente* do Windows em uma máquina virtual para reduzir o uso geral de CPU:

-   Desabilite os serviços em segundo plano, como o SuperFetch e o Windows Search.

-   Desabilite as tarefas agendadas como desfragmentação agendada.

## <a name="virtual-numa"></a>NUMA Virtual

Para habilitar a virtualização de grandes cargas de trabalho de expansão, o Hyper-V no Windows Server 2016 expandido limites de escala de máquina virtual. Pode ser atribuída a uma única máquina virtual até 240 processadores virtuais e 12 TB de memória. Ao criar essas máquinas virtuais grandes, a memória de múltiplos nós no sistema host provavelmente será utilizada. Em tal configuração de máquina virtual, se a memória e processadores virtuais não são alocados no mesmo nó NUMA, as cargas de trabalho podem ter um desempenho ruim devido à incapacidade para tirar proveito das otimizações NUMA.

No Windows Server 2016, o Hyper-V apresenta uma topologia NUMA virtual para máquinas virtuais. Por padrão, esta topologia NUMA virtual é otimizada para corresponder à topologia NUMA do computador host subjacente. Expor uma topologia NUMA virtual em uma máquina virtual permite que o sistema operacional convidado e quaisquer aplicativos habilitados para NUMA, que estejam sendo executados nele, aproveitem as otimizações de desempenho NUMA, da mesma maneira que ocorreria se fossem executados em um computador físico.

Não há distinção entre um NUMA virtual e um físico da perspectiva da carga de trabalho. Dentro de uma máquina virtual, quando uma carga de trabalho aloca memória local para os dados e os acessa no mesmo nó NUMA, o acesso rápido de memória local resulta no sistema físico subjacente. Desta maneira, as penalidades de desempenho causadas pelo acesso de memória remoto são evitadas com sucesso. Somente aplicativos habilitados para NUMA podem se beneficiar de vnuma;.

Microsoft SQL Server é um exemplo de aplicativo com reconhecimento de NUMA. Para obter mais informações, consulte [acesso de memória não uniforme Noções básicas sobre](https://technet.microsoft.com/library/ms178144.aspx).

Os recursos de NUMA virtual e memória dinâmica não podem ser usados ao mesmo tempo. Uma máquina virtual com memória dinâmica habilitada efetivamente possui somente um nó NUMA virtual e nenhuma topologia NUMA é apresentada à máquina virtual, independentemente das configurações de NUMA virtuais.

Para obter mais informações sobre o NUMA Virtual, consulte [visão geral do NUMA Virtual do Hyper-V](https://technet.microsoft.com/library/dn282282.aspx).

## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura Hyper-V](architecture.md)

-   [Configuração do servidor do Hyper-V](configuration.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de E/S de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de E/S de rede do Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
