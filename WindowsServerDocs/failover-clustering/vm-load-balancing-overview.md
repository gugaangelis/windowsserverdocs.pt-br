---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: "Visão geral de balanceamento de carga de máquina virtual"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 0a106db25d476088898b914481e6041f20ce2e9e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-overview"></a>Visão geral de balanceamento de carga de máquina virtual

> Aplica-se a: Windows Server (anual por canal), o Windows Server 2016

Uma consideração importante para implantações de nuvem privada é a capital despesas (<abbr title="gastos de capital">CapEx</abbr>) necessários para entrar em produção. É muito comum para adicionar redundância para implantações de nuvem privada para evitar mudanças em capacidade durante picos de tráfego em produção, mas isso aumenta <abbr title="gastos de capital">CapEx</abbr>. A necessidade de redundância é regida pela desequilibrado nuvens privadas em que alguns nós são hospedagem mais máquinas virtuais (<abbr title="máquinas virtuais">VMs</abbr>) e outros são subutilizada (como um servidor recém-reinicializado).

## <a id="what-is-vm-load-balancing"></a>Qual é o balanceamento de carga de máquina Virtual?
<abbr title="Máquina virtual">VM</abbr> balanceamento de carga é um novo recurso de caixa de entrada no Windows Server 2016 que permite que você otimize a utilização de nós em um Cluster de Failover. Ele identifica os nós excesso comprometidos e distribui novamente <abbr title="máquinas virtuais">VMs</abbr> desses nós para nós sob comprometidos. Alguns dos aspectos destaque desse recurso são da seguinte maneira:

* *É uma solução de inatividade*: <abbr title="máquinas virtuais">VMs</abbr> são migradas ao vivo para nós ociosos.
* *Perfeita integração com o seu ambiente existente do cluster*: políticas de falha, como afinidade anti, domínios de falha e possíveis proprietários são respeitadas.
* *Heurística para equilibrar*: <abbr title="Máquina Virtual">VM</abbr> pressão de memória e a utilização da CPU do nó.
* *Controle granular*: habilitado por padrão. Pode ser ativado por demanda ou em intervalos periódicos.
* *Limites de agressividade*: três limites disponíveis com base nas características da implantação.

## <a id="feature-in-action"></a>O recurso em ação
### <a id="new-node-added"></a>Um novo nó é adicionado ao seu Cluster de Failover
![Gráfico de um novo nó sendo adicionado para o Cluster de Failover](media/vm-load-balancing/overview-VM-load-balancing-1.png)

Quando você adiciona a capacidade de novo para o Cluster de Failover, o <abbr title="máquina virtual">VM</abbr> balanceamento de carga de recurso equilibra automaticamente a capacidade de nós existentes, para o nó recém-adicionado na seguinte ordem:

1. A pressão é avaliada em nós existentes no Cluster de Failover.
2. Todos os nós exceder o limite são identificados.
3. Os nós com a pressão mais alto são identificados para determinar a prioridade de balanceamento.
4. <abbr title="Máquinas virtuais">VMs</abbr> são migrados Live (com sem tempo de inatividade) de um nó exceder o limite para um novo nó adicionado no Cluster de Failover.

### <a id="recurring-load-balancing"></a>Balanceamento de carga recorrente
![Balanceamento de carga do elemento gráfico de uma VM recorrente](media/vm-load-balancing/overview-VM-load-balancing-2.png)

Quando configurado para equilibrar periódicas, a pressão em nós de cluster é avaliada para equilibrar a cada 30 minutos. Como alternativa, a pressão pode ser avaliada sob demanda. Aqui está o fluxo das etapas:

1. A pressão é avaliada em todos os nós na nuvem privada.
2. Todos os nós exceder o limite e aqueles abaixo limite são identificados.
3. Os nós com a pressão mais alto são identificados para determinar a prioridade de balanceamento.
4. <abbr title="Máquinas virtuais">VMs</abbr> são migrados Live (com sem tempo de inatividade) de um nó exceder o limite para o nó sob limite mínimo.

## <a name="see-also"></a>Consulte também
* [Aprofundar de balanceamento de carga de máquina virtual](vm-load-balancing-deep-dive.md)
* [Cluster de failover](failover-clustering-overview.md)
* [Visão geral do Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
