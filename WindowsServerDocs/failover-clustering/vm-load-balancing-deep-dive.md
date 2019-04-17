---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: "Balanceamento de carga de máquina virtual aprofundar"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 6ee092a2e51e2181a2203bc263377c4a8ec60246
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Balanceamento de carga de máquina virtual aprofundar

> Aplica-se a: Windows Server (anual por canal), o Windows Server 2016

Windows Server 2016 apresenta o [recurso balanceamento de carga de máquina Virtual](vm-load-balancing-overview.md) para otimizar a utilização de nós em um Cluster de Failover. Este documento descreve como configurar e controlar <abbr title="máquina virtual">VM</abbr> balanceamento de carga. 

## <a id="heuristics-for-balancing"></a>Heurística para equilibrar
<abbr title="Máquina virtual">VM</abbr> balanceamento de carga de máquina Virtual avalia a carga do nó com base na heurística a seguir:
1. Atual **pressão de memória**: memória é a restrição de recursos mais comuns em um host do Hyper-V
2. <abbr title="Unidade de processamento central">CPU</abbr>**utilização** do nó com base na média ao longo de uma janela de 5 minutos: reduz um nó de cluster se tornando excesso comprometida

## <a id="controlling-aggressiveness-of-balancing"></a>Controlando a agressividade de balanceamento
Agressividade de balanceamento com base na heurística de memória e CPU pode ser configurada usando pela propriedade comuns cluster 'AutoBalancerLevel'. Para controlar a agressividade execute o seguinte no PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Agressividade | Comportamento |
|-------------------|----------------|----------|
| 1 (padrão) | Baixo | Move ao host for maior que 80% carregada |
| 2 | Médio | Move ao host é mais de 70% carregada |
| 3 | Alto | Move ao host é mais de 60% carregada | 

![Gráfico de um PowerShell de Configurando a agressividade de balanceamento](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-abbr-titlevirtual-machinevmabbr-load-balancing"></a>Controlando <abbr title="Máquina Virtual">VM</abbr> balanceamento de carga
<abbr title="Máquina virtual">VM</abbr> balanceamento de carga é habilitada por padrão e balanceamento de carga ocorre quando pode ser configurado, a propriedade comuns do cluster 'AutoBalancerMode'. Para controlar quando a integridade do nó equilibra cluster:

### <a name="using-failover-cluster-manager"></a>Usando o Gerenciador de Cluster de Failover:
1. Clique com botão direito no seu nome de cluster e selecione a opção "Propriedades"  
    ![Gráfico de seleção de propriedade para cluster pelo Gerenciador de Cluster de Failover](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Selecione o painel "Balanceador"  
    ![Gráfico de selecionar a opção balanceador pelo Gerenciador de Cluster de Failover](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>Usando o PowerShell:
Execute o seguinte:
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportamento| 
|:----------------:|:----------:|
|0| Desabilitado| 
|1| Balanceamento de carga em associação de nó| 
|2 (padrão)| Carregar o saldo em associação de nó e cada 30 minutos |

## <a name="abbr-titlevirtual-machinevmabbr-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a><abbr title="Máquina virtual">VM</abbr> balanceamento versus do System Center Virtual Machine Manager dinâmico otimização de carga
O recurso de integridade de nó fornece funcionalidade de caixa de entrada, que é voltada para implantações sem o System Center Virtual Machine Manager (<abbr title="System Center Virtual Machine Manager">SCVMM</abbr>). <abbr title="O System Center Virtual Machine Manager">SCVMM</abbr> otimização dinâmico é o mecanismo recomendado para o balanceamento de carga de máquina virtual no cluster para <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> implantações. <abbr title="O System Center Virtual Machine Manager">SCVMM</abbr> desabilita automaticamente o Windows Server <abbr title="máquina virtual">VM</abbr> balanceamento de carga quando otimização dinâmico está habilitada.

## <a name="see-also"></a>Consulte também
* [Visão geral sobre balanceamento de carga máquina virtual](vm-load-balancing-overview.md)
* [Cluster de failover](failover-clustering-overview.md)
* [Visão geral do Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
