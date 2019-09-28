---
title: Desempenho do processador do Hyper-V
description: Considerações de desempenho do processador no ajuste de desempenho do Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5d61d0e37bd80033bfcfb0cf5c601d8bcedda104
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370045"
---
# <a name="hyper-v-processor-performance"></a>Desempenho do processador do Hyper-V


## <a name="virtual-machine-integration-services"></a>Serviços de integração de máquina virtual

A máquina virtual Integration Services inclui drivers habilitados para dispositivos de e/s específicos do Hyper-V, o que reduz significativamente a sobrecarga de CPU para e/s em comparação com os dispositivos emulados. Você deve instalar a versão mais recente da máquina virtual Integration Services em cada máquina virtual com suporte. Os serviços diminuem o uso da CPU dos convidados, de convidados inativos para convidados muito usados e aumentam a taxa de transferência de e/s. Esta é a primeira etapa no ajuste do desempenho em um servidor que executa o Hyper-V. Para obter uma lista de sistemas operacionais convidados com suporte, consulte [visão geral do Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="virtual-processors"></a>Processadores virtuais

O Hyper-V no Windows Server 2016 dá suporte a um máximo de 240 processadores virtuais por máquina virtual. As máquinas virtuais que têm cargas que não têm uso intensivo de CPU devem ser configuradas para usar um processador virtual. Isso ocorre devido à sobrecarga adicional associada a vários processadores virtuais, como custos de sincronização adicionais no sistema operacional convidado.

Aumente o número de processadores virtuais se a máquina virtual exigir mais de uma CPU de processamento sob carga de pico.

## <a name="background-activity"></a>Atividade em segundo plano

Minimizar a atividade em segundo plano em máquinas virtuais ociosas libera ciclos de CPU que podem ser usados em outro lugar por outras máquinas virtuais. Os convidados do Windows normalmente usam menos de um percentual de uma CPU quando estão ociosos. Veja a seguir várias práticas recomendadas para minimizar o uso de CPU em segundo plano de uma máquina virtual:

-   Instale a versão mais recente da máquina virtual Integration Services.

-   Remova o adaptador de rede emulada por meio da caixa de diálogo Configurações de máquina virtual (use o adaptador específico de Microsoft Hyper-V).

-   Remova os dispositivos não utilizados, como o CD-ROM e a porta COM, ou desconecte suas mídias.

-   Mantenha o sistema operacional convidado do Windows na tela de entrada quando ele não estiver sendo usado e desabilite a proteção de tela.

-   Examine as tarefas e os serviços agendados que estão habilitados por padrão.

-   Examine os provedores de rastreamento ETW que estão ativados por padrão executando o **logman. exe Query-ETS**

-   Melhore os aplicativos de servidor para reduzir atividades periódicas (como temporizadores).

-   Feche Gerenciador do Servidor nos sistemas operacionais host e convidado.

-   Não deixe o Gerenciador do Hyper-V em execução, pois ele atualiza constantemente a miniatura da máquina virtual.

Veja a seguir as práticas recomendadas adicionais para configurar uma *versão de cliente* do Windows em uma máquina virtual para reduzir o uso geral da CPU:

-   Desabilite serviços em segundo plano, como o SuperFetch e o Windows Search.

-   Desabilite as tarefas agendadas, como a desfragmentação agendada.

## <a name="virtual-numa"></a>NUMA Virtual

Para habilitar a virtualização de cargas de trabalho de grande escala, o Hyper-V no Windows Server 2016 os limites de escala de máquina virtual expandida. Uma única máquina virtual pode ser atribuída até 240 processadores virtuais e 12 TB de memória. Ao criar essas máquinas virtuais grandes, a memória de vários nós NUMA no sistema host provavelmente será utilizada. Nessa configuração de máquina virtual, se os processadores virtuais e a memória não forem alocados do mesmo nó NUMA, as cargas de trabalho podem ter um desempenho inadequado devido à incapacidade de tirar proveito das otimizações NUMA.

No Windows Server 2016, o Hyper-V apresenta uma topologia NUMA virtual para máquinas virtuais. Por padrão, esta topologia NUMA virtual é otimizada para corresponder à topologia NUMA do computador host subjacente. Expor uma topologia NUMA virtual em uma máquina virtual permite que o sistema operacional convidado e quaisquer aplicativos habilitados para NUMA, que estejam sendo executados nele, aproveitem as otimizações de desempenho NUMA, da mesma maneira que ocorreria se fossem executados em um computador físico.

Não há nenhuma distinção entre um NUMA virtual e um físico da perspectiva da carga de trabalho. Dentro de uma máquina virtual, quando uma carga de trabalho aloca memória local para os dados e os acessa no mesmo nó NUMA, o acesso rápido de memória local resulta no sistema físico subjacente. Desta maneira, as penalidades de desempenho causadas pelo acesso de memória remoto são evitadas com sucesso. Somente aplicativos com reconhecimento de NUMA podem se beneficiar do vNUMA.

Microsoft SQL Server é um exemplo de aplicativo com reconhecimento de NUMA. Para obter mais informações, consulte [noções básicas sobre o acesso não uniforme à memória](https://technet.microsoft.com/library/ms178144.aspx).

Os recursos de NUMA virtual e memória dinâmica não podem ser usados ao mesmo tempo. Uma máquina virtual com memória dinâmica habilitada efetivamente possui somente um nó NUMA virtual e nenhuma topologia NUMA é apresentada à máquina virtual, independentemente das configurações de NUMA virtuais.

Para obter mais informações sobre NUMA virtual, consulte [visão geral de numa virtual do Hyper-V](https://technet.microsoft.com/library/dn282282.aspx).

## <a name="see-also"></a>Consulte também

-   [Terminologia do Hyper-V](terminology.md)

-   [Arquitetura Hyper-V](architecture.md)

-   [Configuração do servidor do Hyper-V](configuration.md)

-   [Desempenho de memória do Hyper-V](memory-performance.md)

-   [Desempenho de E/S de armazenamento do Hyper-V](storage-io-performance.md)

-   [Desempenho de E/S de rede do Hyper-V](network-io-performance.md)

-   [Detectar gargalos em um ambiente virtualizado](detecting-virtualized-environment-bottlenecks.md)

-   [Máquinas virtuais do Linux](linux-virtual-machine-considerations.md)
