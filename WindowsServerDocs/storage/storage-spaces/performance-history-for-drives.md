---
title: Histórico de desempenho para unidades
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589748"
---
# <a name="performance-history-for-drives"></a>Histórico de desempenho para unidades

> Aplicável à: Demonstração de Insider Windows Server

Este tópico sub-recursos do [histórico de desempenho para espaços de armazenamento direto](performance-history.md) descreve em detalhes o histórico de desempenho coletado para unidades. Histórico de desempenho está disponível para cada unidade no subsistema de armazenamento de cluster, independentemente de barramento ou tipo de mídia. No entanto, ele não está disponível para unidades de inicialização do sistema operacional.

   > [!NOTE]
   > Histórico de desempenho não pode ser coletado para unidades em um servidor que está inoperante. Coleção voltarão automaticamente quando o servidor vem back up.

## <a name="series-names-and-units"></a>Unidades e nomes de série

Essas séries são coletadas para cada unidade elegível:

| Série                          | Unidade             |
|---------------------------------|------------------|
| `physicaldisk.iops.read`        | por segundo       |
| `physicaldisk.iops.write`       | por segundo       |
| `physicaldisk.iops.total`       | por segundo       |
| `physicaldisk.throughput.read`  | bytes por segundo |
| `physicaldisk.throughput.write` | bytes por segundo |
| `physicaldisk.throughput.total` | bytes por segundo |
| `physicaldisk.latency.read`     | segundos          |
| `physicaldisk.latency.write`    | segundos          |
| `physicaldisk.latency.average`  | segundos          |
| `physicaldisk.size.total`       |  bytes            |
| `physicaldisk.size.used`        |  bytes            |

## <a name="how-to-interpret"></a>Como interpretar

| Série                          | Como interpretar                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Número de operações de leitura por segundo concluídas por unidade.                |
| `physicaldisk.iops.write`       | Número de operações de gravação por segundo concluídas por unidade.               |
| `physicaldisk.iops.total`       | Número total de ler ou gravar operações por segundo concluídas por unidade. |
| `physicaldisk.throughput.read`  | Quantidade de dados lidos do disco por segundo.                            |
| `physicaldisk.throughput.write` | Quantidade de dados gravados no disco por segundo.                           |
| `physicaldisk.throughput.total` | Quantidade total de dados lido ou gravado na unidade por segundo.        |
| `physicaldisk.latency.read`     | Latência média das operações de leitura da unidade.                          |
| `physicaldisk.latency.write`    | Latência média das operações de gravação na unidade.                           |
| `physicaldisk.latency.average`  | Latência média de todas as operações de ou para a unidade.                     |
| `physicaldisk.size.total`       | A capacidade de armazenamento total da unidade.                                    |
| `physicaldisk.size.used`        | A capacidade de armazenamento usado da unidade.                                     |

## <a name="where-they-come-from"></a>Onde eles vêm

O `iops.*`, `throughput.*`, e `latency.*` série será coletada a partir do `Physical Disk` contador de desempenho definidas no servidor em que a unidade está conectada, uma instância por unidade. Esses contadores são medidos por `partmgr.sys` e não incluem quantidade da pilha de software do Windows nem qualquer saltos de rede. Eles são representativo de desempenho de hardware do dispositivo.

| Série                          | Contador de origem           |
|---------------------------------|--------------------------|
| `physicaldisk.iops.read`        | `Disk Reads/sec`         |
| `physicaldisk.iops.write`       | `Disk Writes/sec`        |
| `physicaldisk.iops.total`       | `Disk Transfers/sec`     |
| `physicaldisk.throughput.read`  | `Disk Read Bytes/sec`    |
| `physicaldisk.throughput.write` | `Disk Write Bytes/sec`   |
| `physicaldisk.throughput.total` | `Disk Bytes/sec`         |
| `physicaldisk.latency.read`     | `Avg. Disk sec/Read`     |
| `physicaldisk.latency.write`    | `Avg. Disk sec/Writes`   |
| `physicaldisk.latency.average`  | `Avg. Disk sec/Transfer` |

   > [!NOTE]
   > Contadores são medidos durante o intervalo inteiro, não de amostra. Por exemplo, se a unidade estiver ociosa por 9 segundos mas conclui 30 IOs na segunda 10º, seu `physicaldisk.iops.total` serão registradas como 3 IOs por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura todas as atividades e é robusto para ruído.

O `size.*` série será coletada a partir do `MSFT_PhysicalDisk` classe WMI, uma instância por unidade.

| Série                          | Propriedade Source        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) :

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para espaços de armazenamento direto](performance-history.md)
