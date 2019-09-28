---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: Visão geral do balanceamento de carga da máquina virtual
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 1fea9e6297399a5081eb8fef1876f1b11d23c745
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360983"
---
# <a name="virtual-machine-load-balancing-overview"></a>Visão geral do balanceamento de carga da máquina virtual

> Aplica-se a: Windows Server 2019, Windows Server 2016

Uma consideração importante para implantações de nuvem privada é a despesa de capital (<abbr title="despesas de capital">CapEx</abbr>) necessário para entrar em produção. É muito comum adicionar redundância a implantações de nuvem privada para evitar a capacidade durante o tráfego de pico na produção, mas isso aumenta <abbr title="despesas de capital">CapEx</abbr>. A necessidade de redundância é orientada por nuvens privadas desbalanceadas em que alguns nós estão hospedando mais máquinas virtuais (<abbr title="máquinas virtuais">VMs</abbr>) e outros são subutilizados (como um novo servidor reinicializado).

<strong>Visão geral em vídeo rápido</strong><br>(6 minutos)<br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a id="what-is-vm-load-balancing"></a>O que é balanceamento de carga de máquina virtual?
<abbr title="Máquina virtual">VM</abbr> O balanceamento de carga é um recurso integrado no Windows Server 2019 e no Windows Server 2016 que permite otimizar a utilização de nós em um cluster de failover. Ele identifica nós com confirmação excessiva e distribui novamente <abbr title="máquinas virtuais">VMs</abbr> desses nós para nós sob confirmação. Alguns dos aspectos evidentes desse recurso são os seguintes:

* *É uma solução de tempo de inatividade zero*: <abbr title="Máquinas Virtuais">VMs</abbr> são migrados ao vivo para nós ociosos.
* *Integração direta com seu ambiente de cluster existente*: As políticas de falha, como antiafinidade, domínios de falha e possíveis proprietários, são respeitadas.
* *Heurística para balanceamento*: <abbr title="Máquina virtual">VM</abbr> pressão de memória e utilização de CPU do nó.
* *Controle granular*: Habilitado por padrão. Pode ser ativado sob demanda ou em um intervalo periódico.
* *Limites de agressividade*: Três limites disponíveis com base nas características da sua implantação.

## <a id="feature-in-action"></a>O recurso em ação
### <a id="new-node-added"></a>Um novo nó é adicionado ao cluster de failover
![Gráfico de um novo nó que está sendo adicionado ao cluster de failover](media/vm-load-balancing/overview-VM-load-balancing-1.png)

Quando você adiciona uma nova capacidade ao cluster de failover do, o <abbr title="Máquina virtual">VM</abbr> O recurso de balanceamento de carga equilibra automaticamente a capacidade dos nós existentes para o nó recém-adicionado na seguinte ordem:

1. A pressão é avaliada nos nós existentes no cluster de failover.
2. Todos os nós excedendo o limite são identificados.
3. Os nós com a pressão mais alta são identificados para determinar a prioridade do balanceamento.
4. <abbr title="Máquinas Virtuais">VMs</abbr> são migradas ao vivo (sem tempo de inatividade) de um nó que excede o limite para um nó recém-adicionado no cluster de failover.

### <a id="recurring-load-balancing"></a>Balanceamento de carga recorrente
![Gráfico de um balanceamento de carga de VM recorrente](media/vm-load-balancing/overview-VM-load-balancing-2.png)

Quando configurado para o balanceamento periódico, a pressão nos nós do cluster é avaliada para balanceamento a cada 30 minutos. Como alternativa, a pressão pode ser avaliada sob demanda. Este é o fluxo das etapas:

1. A pressão é avaliada em todos os nós na nuvem privada.
2. Todos os nós excederam o limite e os limites abaixo são identificados.
3. Os nós com a pressão mais alta são identificados para determinar a prioridade do balanceamento.
4. <abbr title="Máquinas Virtuais">VMs</abbr> são migradas ao vivo (sem tempo de inatividade) de um nó que excede o limite para o nó abaixo do limite mínimo.

## <a name="see-also"></a>Consulte também
* [Aprofundamento de balanceamento de carga de máquina virtual](vm-load-balancing-deep-dive.md)
* [Clustering de failover](failover-clustering-overview.md)
* [Visão geral do Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
