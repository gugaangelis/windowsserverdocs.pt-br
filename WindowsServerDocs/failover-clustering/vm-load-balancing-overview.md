---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: Visão geral do balanceamento de carga de máquina virtual
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 125dd7421cc1876c07983016498a9689d8a507ac
ms.sourcegitcommit: a3c9a7718502de723e8c156288017de465daaf6b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2019
ms.locfileid: "65475984"
---
# <a name="virtual-machine-load-balancing-overview"></a>Visão geral do balanceamento de carga de máquina virtual

> Aplica-se a: Windows Server 2019, Windows Server 2016

Uma consideração importante para implantações de nuvem privada é o (despesas de capital<abbr title="despesas de capital">CapEx</abbr>) necessária para entrar em produção. É muito comum para adicionar redundância em implantações de nuvem privada para evitar a capacidade insuficiente durante picos de tráfego em produção, mas isso aumenta <abbr title="despesas de capital">CapEx</abbr>. A necessidade de redundância é orientada pelo desbalanceada nuvens privadas em que alguns nós hospeda (mais de máquinas virtuais<abbr title="máquinas virtuais">VMs</abbr>) e outras pessoas estão subutilizadas (como um servidor reinicializado recentemente).

<strong>Visão geral rápida do vídeo</strong><br>(6 minutos)<br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a id="what-is-vm-load-balancing"></a>O que é balanceamento de carga com a máquina Virtual?
<abbr title="Máquina virtual">VM</abbr> Balanceamento de carga é um recurso de caixa de entrada no Windows Server 2019 e Windows Server 2016 que permite que você otimize a utilização de nós em um Cluster de Failover. Ele identifica os nós de excesso de comprometimento e distribui novamente <abbr title="máquinas virtuais">VMs</abbr> de em nós para nós sob confirmada. Alguns dos principais aspectos desse recurso são da seguinte maneira:

* *É uma solução sem tempo de inatividade*: <abbr title="Máquinas Virtuais">VMs</abbr> são migradas ao vivo para nós ociosos.
* *Integração perfeita com o ambiente de cluster existente*: Diretivas de falha como antiafinidade, domínios de falha e possíveis proprietários sejam respeitadas.
* *Heurística para balanceamento*: <abbr title="Máquina virtual">VM</abbr> pressão de memória e utilização da CPU do nó.
* *Controle granular*: Habilitado por padrão. Pode ser ativado sob demanda ou em um intervalo periódico.
* *Os limites de agressividade*: Três limites disponíveis com base nas características da sua implantação.

## <a id="feature-in-action"></a>O recurso em ação
### <a id="new-node-added"></a>Um novo nó é adicionado ao seu Cluster de Failover
![Gráfico de um novo nó que está sendo adicionado ao seu Cluster de Failover](media/vm-load-balancing/overview-VM-load-balancing-1.png)

Quando você adiciona a nova capacidade para o Cluster de Failover, o <abbr title="máquina virtual">VM</abbr> Recurso de balanceamento de carga equilibra automaticamente a capacidade dos nós existentes, para o nó recém-adicionado na seguinte ordem:

1. A pressão é avaliada em nós existentes no Cluster de Failover.
2. Todos os nós que excedem o limite são identificados.
3. Os nós com a mais alta pressão são identificados para determinar a prioridade de balanceamento.
4. <abbr title="Máquinas Virtuais">VMs</abbr> são migradas ao vivo (sem nenhum tempo de inatividade) de um nó que excedem o limite para um nó recém-adicionado no Cluster de Failover.

### <a id="recurring-load-balancing"></a>Balanceamento de carga recorrente
![Gráfico de uma VM recorrente o balanceamento de carga](media/vm-load-balancing/overview-VM-load-balancing-2.png)

Quando configurado para balanceamento periódica, a pressão em nós de cluster é avaliada para balanceamento a cada 30 minutos. Como alternativa, a pressão pode ser avaliadas sob demanda. Aqui está o fluxo das etapas:

1. A pressão é avaliada em todos os nós na nuvem privada.
2. Todos os nós que excedem o limite e aquelas abaixo do limite são identificados.
3. Os nós com a mais alta pressão são identificados para determinar a prioridade de balanceamento.
4. <abbr title="Máquinas Virtuais">VMs</abbr> são migradas ao vivo (sem nenhum tempo de inatividade) de um nó que excedem o limite para o nó abaixo do limite mínimo.

## <a name="see-also"></a>Consulte também
* [Aprofundamento do balanceamento de carga de máquina virtual](vm-load-balancing-deep-dive.md)
* [Clustering de failover](failover-clustering-overview.md)
* [Visão geral do Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
