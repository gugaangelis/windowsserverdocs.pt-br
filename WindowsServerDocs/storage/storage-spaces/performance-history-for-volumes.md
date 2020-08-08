---
title: Histórico de desempenho de volumes
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 367866739f18e426ad7c61dba3dac60b12692bbe
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954643"
---
# <a name="performance-history-for-volumes"></a>Histórico de desempenho de volumes

> Aplica-se a: Windows Server 2019

Este subtópico do [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para volumes. O histórico de desempenho está disponível para cada Volume Compartilhado Clusterizado (CSV) no cluster. No entanto, ele não está disponível para volumes de inicialização do sistema operacional nem para nenhum outro armazenamento não CSV.

   > [!NOTE]
   > Pode levar vários minutos para que a coleta comece para volumes recém-criados ou renomeados.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para cada volume elegível:

| Série                    | Unidade             |
|---------------------------|------------------|
| `volume.iops.read`        | por segundo       |
| `volume.iops.write`       | por segundo       |
| `volume.iops.total`       | por segundo       |
| `volume.throughput.read`  | bytes por segundo |
| `volume.throughput.write` | bytes por segundo |
| `volume.throughput.total` | bytes por segundo |
| `volume.latency.read`     | segundos          |
| `volume.latency.write`    | segundos          |
| `volume.latency.average`  | segundos          |
| `volume.size.total`       | bytes            |
| `volume.size.available`   | bytes            |

## <a name="how-to-interpret"></a>Como interpretar

| Série                    | Como interpretar                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Número de operações de leitura por segundo concluídas por este volume.                |
| `volume.iops.write`       | Número de operações de gravação por segundo concluídas por este volume.               |
| `volume.iops.total`       | Número total de operações de leitura ou gravação por segundo concluídas por este volume. |
| `volume.throughput.read`  | Quantidade de dados lidos deste volume por segundo.                            |
| `volume.throughput.write` | Quantidade de dados gravados neste volume por segundo.                           |
| `volume.throughput.total` | Quantidade total de dados lidos ou gravados neste volume por segundo.        |
| `volume.latency.read`     | Latência média de operações de leitura deste volume.                          |
| `volume.latency.write`    | Latência média de operações de gravação para este volume.                           |
| `volume.latency.average`  | Latência média de todas as operações para ou deste volume.                     |
| `volume.size.total`       | A capacidade de armazenamento total do volume.                                     |
| `volume.size.available`   | A capacidade de armazenamento disponível do volume.                                 |

## <a name="where-they-come-from"></a>De onde vêm

As `iops.*` `throughput.*` séries, e `latency.*` são coletadas do `Cluster CSVFS` conjunto de contadores de desempenho. Cada servidor no cluster tem uma instância para cada volume CSV, independentemente da propriedade. O histórico de desempenho registrado para o volume `MyVolume` é a agregação das `MyVolume` instâncias em cada servidor no cluster.

| Série                    | Contador de origem         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *soma dos itens acima*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *soma dos itens acima*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *média dos itens acima* |

   > [!NOTE]
   > Os contadores são medidos em todo o intervalo, não amostras. Por exemplo, se o volume estiver ocioso por 9 segundos, mas concluir 30 IOs em 10 de segundo, seu `volume.iops.total` será registrado como 3 Ios por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho Capture todas as atividades e seja robusto para ruído.

   > [!TIP]
   > Esses são os mesmos contadores usados pela estrutura de benchmark de [frota de VM](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) popular.

A `size.*` série é coletada da `MSFT_Volume` classe no WMI, uma instância por volume.

| Série                    | propriedade Source |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-volume](/powershell/module/storage/get-volume) :

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="additional-references"></a>Referências adicionais

- [Histórico de desempenho para Espaços de Armazenamento Diretos](performance-history.md)
