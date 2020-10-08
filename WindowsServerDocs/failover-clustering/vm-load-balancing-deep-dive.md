---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: Aprofundamento de balanceamento de carga de máquina virtual
ms.topic: article
manager: eldenc
ms.author: johnmar
author: JasonGerend
ms.date: 09/19/2016
ms.openlocfilehash: 7fc9b449b11b5faf05ac279628f093053e292e8c
ms.sourcegitcommit: 7a8a608df059b4278a974c52ed7b865421a83aa6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91833308"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Aprofundamento de balanceamento de carga de máquina virtual

> Aplica-se a: Windows Server 2019, Windows Server 2016

O [recurso de balanceamento de carga de máquina virtual](vm-load-balancing-overview.md) otimiza a utilização de nós em um cluster de failover. Este documento descreve como configurar e controlar o balanceamento de carga de VM.

## <a name="heuristics-for-balancing"></a><a id="heuristics-for-balancing"></a>Heurística para balanceamento
O balanceamento de carga da máquina virtual avalia a carga de um nó com base na heurística a seguir:
1. **Pressão de memória**atual: a memória é a restrição de recursos mais comum em um host Hyper-V
2. **Utilização** da CPU do nó calculada por média em uma janela de 5 minutos: atenua um nó no cluster ficando excessivamente confirmado

## <a name="controlling-the-aggressiveness-of-balancing"></a><a id="controlling-aggressiveness-of-balancing"></a>Controlando a agressividade do balanceamento
A agressividade de balanceamento com base na memória e na heurística da CPU pode ser configurada usando o pela propriedade comum do cluster ' AutoBalancerLevel '. Para controlar a agressividade, execute o seguinte no PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Agressividade | Comportamento |
|-------------------|----------------|----------|
| 1 (padrão) | Baixo | Mover quando o host tiver mais de 80% carregado |
| 2 | Médio | Mover quando o host tiver mais de 70% carregado |
| 3 | Alto | Média de nós e movimentação quando o host é superior a 5% acima da média |

![Gráfico de um PowerShell de configuração da agressividade de balanceamento](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>Controlando o balanceamento de carga de VM
O balanceamento de carga de VM é habilitado por padrão e quando ocorre o balanceamento de carga pode ser configurado pela propriedade comum de cluster ' AutoBalancerMode '. Para controlar quando a imparcialção de nó balanceia o cluster:

### <a name="using-failover-cluster-manager"></a>Usando Gerenciador de Cluster de Failover:
1. Clique com o botão direito do mouse no nome do cluster e selecione o  ![ elemento gráfico da opção "Propriedades" da seleção de propriedade para cluster por meio de Gerenciador de cluster de failover](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Selecione o gráfico do painel "balanceador" para ![ selecionar a opção de balanceador por meio de Gerenciador de cluster de failover](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>Usando o PowerShell:
Execute o seguinte:
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportamento|
|:----------------:|:----------:|
|0| Desabilitado|
|1| Balanceamento de carga na junção de nó|
|2 (padrão)| Balanceamento de carga em junção de nó e a cada 30 minutos |

## <a name="vm-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a>Balanceamento de carga de VM versus System Center Virtual Machine Manager otimização dinâmica
O recurso de imparcialidade de nó fornece funcionalidade na caixa, direcionada para implantações sem System Center Virtual Machine Manager (SCVMM). A otimização dinâmica do SCVMM é o mecanismo recomendado para balancear a carga da máquina virtual em seu cluster para implantações do SCVMM. O SCVMM desabilita automaticamente o balanceamento de carga de VM do Windows Server quando a otimização dinâmica está habilitada.

## <a name="see-also"></a>Consulte Também
* [Visão geral do balanceamento de carga da máquina virtual](vm-load-balancing-overview.md)
* [Clustering de failover](failover-clustering-overview.md)
* [Visão geral do Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
