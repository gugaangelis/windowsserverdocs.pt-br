---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: Balanceamento de carga com a máquina virtual de aprofundamento
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: f90802a8e77ade0b9f282730e7cb61c73a246018
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853417"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Balanceamento de carga com a máquina virtual de aprofundamento

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Windows Server 2016 apresenta o [recurso de balanceamento de carga com a máquina Virtual](vm-load-balancing-overview.md) para otimizar a utilização de nós em um Cluster de Failover. Este documento descreve como configurar e controlar <abbr title="máquina virtual">VM</abbr> balanceamento de carga. 

## <a id="heuristics-for-balancing"></a>Heurística de balanceamento
<abbr title="máquina virtual">VM</abbr> balanceamento de carga de máquina Virtual avalia a carga de um nó com base na heurística a seguir:
1. Atual **pressão de memória**: A memória é a restrição de recursos mais comuns em um host Hyper-V
2. <abbr title="Unidade de processamento central">CPU</abbr> **utilização** do nó média de uma janela de 5 minutos: Atenua um nó do cluster se torne o excesso de comprometimento

## <a id="controlling-aggressiveness-of-balancing"></a>Controlando a agressividade de balanceamento
A agressividade de balanceamento com base na heurística de memória e CPU pode ser configurada usando pela propriedade de cluster comum 'AutoBalancerLevel'. Para controlar a agressividade, execute o seguinte no PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Agressividade | Comportamento |
|-------------------|----------------|----------|
| 1 (padrão) | Baixo | Mover quando o host é mais de 80% carregado |
| 2 | Médio | Mover quando o host for mais de 70% carregado |
| 3 | Alto | Média de nós e mover ao host é mais de 5% acima da média | 

![Gráfico do PowerShell de configuração a agressividade de balanceamento](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-abbr-titlevirtual-machinevmabbr-load-balancing"></a>Controlando <abbr title="Máquina Virtual">VM</abbr> balanceamento de carga
<abbr title="máquina virtual">VM</abbr> balanceamento de carga é habilitado por padrão e quando ocorre o balanceamento de carga pode ser configurado com a propriedade comum 'AutoBalancerMode' do cluster. Para controlar quando os saldos de Equidade de nó do cluster:

### <a name="using-failover-cluster-manager"></a>Usando o Gerenciador de Cluster de Failover:
1. Clique com botão direito no nome do seu cluster e selecione a opção de "Propriedades"  
    ![Gráfico de seleção de propriedade para o cluster por meio do Gerenciador de Cluster de Failover](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Selecione o painel de "Balanceador"  
    ![Gráfico de selecionar a opção balanceador por meio do Gerenciador de Cluster de Failover](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>Usando o PowerShell:
Execute o seguinte:
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportamento| 
|:----------------:|:----------:|
|0| Desabilitada| 
|1| Balancear carga em associação do nó| 
|2 (padrão)| Na associação de nó e a cada 30 minutos de balanceamento de carga |

## <a name="abbr-titlevirtual-machinevmabbr-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a><abbr title="Máquina virtual">VM</abbr> vs de balanceamento de carga. System Center Virtual Machine Manager dinâmico otimização
O recurso de integridade do nó, fornece funcionalidade de caixa de entrada, que é destinada à implantações sem o System Center Virtual Machine Manager (<abbr title="do System Center Virtual Machine Manager">SCVMM</abbr>). <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> otimização dinâmica é o mecanismo recomendado para o balanceamento de carga de máquinas virtuais no cluster para <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> implantações. <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> desabilita automaticamente o Windows Server <abbr title="máquina virtual">VM</abbr> balanceamento de carga quando a otimização dinâmica está habilitada.

## <a name="see-also"></a>Consulte também
* [Visão geral de balanceamento de carga máquina virtual](vm-load-balancing-overview.md)
* [Clustering de failover](failover-clustering-overview.md)
* [Visão geral do Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
