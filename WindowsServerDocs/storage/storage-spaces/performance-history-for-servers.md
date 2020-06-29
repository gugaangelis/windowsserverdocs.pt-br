---
title: Histórico de desempenho para servidores
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7ed910aba376ba7f78c628d7f47bdd21b366459d
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474733"
---
# <a name="performance-history-for-servers"></a>Histórico de desempenho para servidores

> Aplica-se a: Windows Server 2019

Este subtópico do [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para servidores. O histórico de desempenho está disponível para cada servidor no cluster.

   > [!NOTE]
   > O histórico de desempenho não pode ser coletado para um servidor que está inoperante. A coleta será retomada automaticamente quando o servidor voltar a ficar em funcionamento.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para todos os servidores qualificados:

| Série                           | Unidade    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | {1&gt;percent&lt;1} |
| `clusternode.cpu.usage.guest`    | {1&gt;percent&lt;1} |
| `clusternode.cpu.usage.host`     | {1&gt;percent&lt;1} |
| `clusternode.memory.total`       | bytes   |
| `clusternode.memory.available`   | bytes   |
| `clusternode.memory.usage`       | bytes   |
| `clusternode.memory.usage.guest` | bytes   |
| `clusternode.memory.usage.host`  | bytes   |

Além disso, as séries de unidades, como `physicaldisk.size.total` são agregadas para todas as unidades qualificadas anexadas ao servidor, e a série de adaptadores de rede, como `networkadapter.bytes.total` são agregadas para todos os adaptadores de rede qualificados anexados ao servidor.

## <a name="how-to-interpret"></a>Como interpretar

| Série                           | Como interpretar                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Percentual de tempo do processador que não está ocioso.                        |
| `clusternode.cpu.usage.guest`    | Percentual do tempo do processador usado para a demanda de convidado (máquina virtual). |
| `clusternode.cpu.usage.host`     | Percentual do tempo do processador usado para a demanda do host.                    |
| `clusternode.memory.total`       | A memória física total do servidor.                              |
| `clusternode.memory.available`   | A memória disponível do servidor.                                   |
| `clusternode.memory.usage`       | A memória alocada (não disponível) do servidor.                   |
| `clusternode.memory.usage.guest` | A memória alocada para a demanda de convidado (máquina virtual).               |
| `clusternode.memory.usage.host`  | A memória alocada para hospedar a demanda.                                  |

## <a name="where-they-come-from"></a>De onde vêm

A `cpu.*` série é coletada de diferentes contadores de desempenho, dependendo se o Hyper-V está habilitado ou não.

Se o Hyper-V estiver habilitado:

| Série                           | Contador de origem |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

O uso dos `% Total Run Time` contadores garante que todos os atributos de histórico de desempenho tenham todos os usos.

Se o Hyper-V não estiver habilitado:

| Série                           | Contador de origem |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zero* |
| `clusternode.cpu.usage.host`     | *igual ao uso total* |

Não obstante a sincronização imperfeita, `clusternode.cpu.usage` é sempre `clusternode.cpu.usage.host` mais `clusternode.cpu.usage.guest` .

Com a mesma limitação, `clusternode.cpu.usage.guest` sempre é a soma de `vm.cpu.usage` todas as máquinas virtuais no servidor host.

A `memory.*` série será (em breve).

  > [!NOTE]
  > Os contadores são medidos em todo o intervalo, não amostras. Por exemplo, se o servidor estiver ocioso por 9 segundos, mas picos para 100% da CPU no décimo segundo, seu `clusternode.cpu.usage` será registrado como 10% em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho Capture todas as atividades e seja robusto para ruído.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) :

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="additional-references"></a>Referências adicionais

- [Histórico de desempenho para Espaços de Armazenamento Diretos](performance-history.md)
