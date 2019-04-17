---
title: Histórico de desempenho para servidores
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894245"
---
# <a name="performance-history-for-servers"></a>Histórico de desempenho para servidores

> Aplicável à: Demonstração de Insider Windows Server

Este tópico sub-recursos do [histórico de desempenho para espaços de armazenamento direto](performance-history.md) descreve em detalhes o histórico de desempenho coletado para servidores. Histórico de desempenho está disponível para cada servidor do cluster.

   > [!NOTE]
   > Histórico de desempenho não pode ser coletado para um servidor que está inoperante. Coleção voltarão automaticamente quando o servidor vem back up.

## <a name="series-names-and-units"></a>Unidades e nomes de série

Essas séries são coletadas para cada servidor elegível:

| Série                           | Unidade    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | % |
| `clusternode.cpu.usage.guest`    | % |
| `clusternode.cpu.usage.host`     | % |
| `clusternode.memory.total`       |  bytes   |
| `clusternode.memory.available`   |  bytes   |
| `clusternode.memory.usage`       |  bytes   |
| `clusternode.memory.usage.guest` |  bytes   |
| `clusternode.memory.usage.host`  |  bytes   |

Além disso, unidade série como `physicaldisk.size.total` são agregadas para todas as unidades elegíveis anexadas ao servidor e série de adaptador de rede como `networkadapter.bytes.total` são agregadas para todos os adaptadores de rede elegíveis conectados ao servidor.

## <a name="how-to-interpret"></a>Como interpretar

| Série                           | Como interpretar                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Porcentagem de tempo do processador que não está ocioso.                        |
| `clusternode.cpu.usage.guest`    | Porcentagem de tempo do processador usado para propostas de convidado (máquina virtual). |
| `clusternode.cpu.usage.host`     | Porcentagem de tempo do processador usado para propostas de host.                    |
| `clusternode.memory.total`       | A memória física total do servidor.                              |
| `clusternode.memory.available`   | A memória disponível do servidor.                                   |
| `clusternode.memory.usage`       | A memória alocada (não disponível) do servidor.                   |
| `clusternode.memory.usage.guest` | A memória alocada para propostas de convidado (máquina virtual).               |
| `clusternode.memory.usage.host`  | A memória alocada para propostas de host.                                  |

## <a name="where-they-come-from"></a>Onde eles vêm

O `cpu.*` série coletada dos contadores de desempenho diferentes dependendo se é ou não o Hyper-V está habilitado.

Se o Hyper-V está habilitado:

| Série                           | Contador de origem |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

Usando o `% Total Run Time` contadores garante que o histórico de desempenho atributos de uso de todos os.

Se não estiver habilitado o Hyper-V:

| Série                           | Contador de origem |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zero* |
| `clusternode.cpu.usage.host`     | *mesmo que o uso total* |

Despeito sincronização imperfeita, `clusternode.cpu.usage` é sempre `clusternode.cpu.usage.host` plus `clusternode.cpu.usage.guest`.

Com o mesmo senão, `clusternode.cpu.usage.guest` sempre é a soma de `vm.cpu.usage` para todas as máquinas virtuais no servidor host.

O `memory.*` série é (LANÇADO em breve).

  > [!NOTE]
  > Contadores são medidos durante o intervalo inteiro, não de amostra. Por exemplo, se o servidor está ocioso por 9 segundos, mas picos de até 100% da CPU no segundo 10º, seu `clusternode.cpu.usage` serão registradas como 10% em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura todas as atividades e é robusto para ruído.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) :

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para espaços de armazenamento direto](performance-history.md)
