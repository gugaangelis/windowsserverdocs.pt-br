---
title: Histórico de desempenho para servidores
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: Espaços de Armazenamento Diretos
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820587"
---
# <a name="performance-history-for-servers"></a>Histórico de desempenho para servidores

> Aplica-se a: Windows Server Insider Preview

Este tópico subpropriedades de [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para servidores. Histórico de desempenho está disponível para todos os servidores no cluster.

   > [!NOTE]
   > Histórico de desempenho não pode ser coletado para um servidor que está inativo. Coleção será retomada automaticamente quando o servidor de volta a funcionar.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para todos os servidores qualificados:

| série                           | Unidade    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | Por cento |
| `clusternode.cpu.usage.guest`    | Por cento |
| `clusternode.cpu.usage.host`     | Por cento |
| `clusternode.memory.total`       | bytes   |
| `clusternode.memory.available`   | bytes   |
| `clusternode.memory.usage`       | bytes   |
| `clusternode.memory.usage.guest` | bytes   |
| `clusternode.memory.usage.host`  | bytes   |

Além disso, unidade, como a série `physicaldisk.size.total` são agregados para todas as unidades qualificadas anexadas para o servidor e a série de adaptador de rede, como `networkadapter.bytes.total` são agregados para todos os adaptadores de rede elegíveis anexados ao servidor.

## <a name="how-to-interpret"></a>Como interpretar

| série                           | Como interpretar                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Porcentagem de tempo do processador não ocioso.                        |
| `clusternode.cpu.usage.guest`    | Porcentagem de tempo de processador usado para a demanda de convidado (máquina virtual). |
| `clusternode.cpu.usage.host`     | Porcentagem de tempo de processador usado para as demandas de host.                    |
| `clusternode.memory.total`       | A memória física total do servidor.                              |
| `clusternode.memory.available`   | A memória disponível do servidor.                                   |
| `clusternode.memory.usage`       | A memória alocada (não disponível) do servidor.                   |
| `clusternode.memory.usage.guest` | A memória alocada à demanda de convidado (máquina virtual).               |
| `clusternode.memory.usage.host`  | A memória alocada à demanda de host.                                  |

## <a name="where-they-come-from"></a>Onde eles vêm

O `cpu.*` série é coletada de contadores de desempenho diferentes, dependendo de se ou não Hyper-V está habilitado.

Se o Hyper-V está habilitado:

| série                           | Contador de origem |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

Usando o `% Total Run Time` contadores garante que todo o uso de atributos histórico de desempenho.

Se o Hyper-V não está habilitado:

| série                           | Contador de origem |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zero* |
| `clusternode.cpu.usage.host`     | *mesmo que o uso total* |

Não obstante sincronização imperfeita, `clusternode.cpu.usage` é sempre `clusternode.cpu.usage.host` plus `clusternode.cpu.usage.guest`.

Com a mesma limitação, `clusternode.cpu.usage.guest` sempre é a soma de `vm.cpu.usage` todas as máquinas virtuais no servidor host.

O `memory.*` séries são (em breve).

  > [!NOTE]
  > Contadores são medidos ao longo de todo o intervalo, não amostrado. Por exemplo, se o servidor está ocioso para 9 segundos, mas picos de até 100% da CPU na segunda, 10 de seus `clusternode.cpu.usage` serão registradas como 10% em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura todas as atividades e é robusto para o ruído.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) cmdlet:

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para espaços de armazenamento diretos](performance-history.md)
