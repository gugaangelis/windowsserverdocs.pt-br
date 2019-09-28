---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: Aprofundamento de balanceamento de carga de máquina virtual
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 972e86d5f49f92d090eed1d4130544d0269c1309
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392226"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Aprofundamento de balanceamento de carga de máquina virtual

> Aplica-se a: Windows Server 2019, Windows Server 2016

O [recurso de balanceamento de carga de máquina virtual](vm-load-balancing-overview.md) otimiza a utilização de nós em um cluster de failover. Este documento descreve como configurar e controlar o balanceamento de carga de VM. 

## <a id="heuristics-for-balancing"></a>Heurística para balanceamento
O balanceamento de carga da máquina virtual avalia a carga de um nó com base na heurística a seguir:
1. **Pressão de memória**atual: A memória é a restrição de recursos mais comum em um host Hyper-V
2. A **utilização** da CPU do nó é calculada por média em uma janela de 5 minutos: Atenua o excesso de confirmação de um nó no cluster

## <a id="controlling-aggressiveness-of-balancing"></a>Controlando a agressividade do balanceamento
A agressividade de balanceamento com base na memória e na heurística da CPU pode ser configurada usando o pela propriedade comum do cluster ' AutoBalancerLevel '. Para controlar a agressividade, execute o seguinte no PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Agressividade | Comportamento |
|-------------------|----------------|----------|
| 1 (padrão) | Baixa | Mover quando o host tiver mais de 80% carregado |
| 2 | Média | Mover quando o host tiver mais de 70% carregado |
| 3 | Alto | Média de nós e movimentação quando o host é superior a 5% acima da média | 

![Gráfico de um PowerShell de configuração da agressividade de balanceamento](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>Controlando o balanceamento de carga de VM
O balanceamento de carga de VM é habilitado por padrão e quando ocorre o balanceamento de carga pode ser configurado pela propriedade comum de cluster ' AutoBalancerMode '. Para controlar quando a imparcialção de nó balanceia o cluster:

### <a name="using-failover-cluster-manager"></a>Usando Gerenciador de Cluster de Failover:
1. Clique com o botão direito do mouse no nome do cluster e selecione a opção "Propriedades"  
    ![Gráfico de seleção de propriedade para cluster por meio de Gerenciador de Cluster de Failover](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Selecione o painel "balanceador"  
    ![Gráfico de seleção da opção de balanceador por meio de Gerenciador de Cluster de Failover](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

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

## <a name="see-also"></a>Consulte também
* [Visão geral do balanceamento de carga da máquina virtual](vm-load-balancing-overview.md)
* [Clustering de failover](failover-clustering-overview.md)
* [Visão geral do Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
