---
title: Histórico de desempenho para unidades
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7e1620f7010d4f37713de20f2b4c12f100be61dc
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474763"
---
# <a name="performance-history-for-drives"></a>Histórico de desempenho para unidades

> Aplica-se a: Windows Server 2019

Este subtópico do [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para unidades. O histórico de desempenho está disponível para cada unidade no subsistema de armazenamento de cluster, independentemente do tipo de barramento ou de mídia. No entanto, ele não está disponível para unidades de inicialização do sistema operacional.

   > [!NOTE]
   > O histórico de desempenho não pode ser coletado para unidades em um servidor que está inoperante. A coleta será retomada automaticamente quando o servidor voltar a ficar em funcionamento.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para cada unidade qualificada:

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
| `physicaldisk.size.total`       | bytes            |
| `physicaldisk.size.used`        | bytes            |

## <a name="how-to-interpret"></a>Como interpretar

| Série                          | Como interpretar                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Número de operações de leitura por segundo concluídas pela unidade.                |
| `physicaldisk.iops.write`       | Número de operações de gravação por segundo concluídas pela unidade.               |
| `physicaldisk.iops.total`       | Número total de operações de leitura ou gravação por segundo concluídas pela unidade. |
| `physicaldisk.throughput.read`  | Quantidade de dados lidos da unidade por segundo.                            |
| `physicaldisk.throughput.write` | Quantidade de dados gravados na unidade por segundo.                           |
| `physicaldisk.throughput.total` | Quantidade total de dados lidos ou gravados na unidade por segundo.        |
| `physicaldisk.latency.read`     | Latência média de operações de leitura da unidade.                          |
| `physicaldisk.latency.write`    | Latência média de operações de gravação na unidade.                           |
| `physicaldisk.latency.average`  | Latência média de todas as operações de ou para a unidade.                     |
| `physicaldisk.size.total`       | A capacidade de armazenamento total da unidade.                                    |
| `physicaldisk.size.used`        | A capacidade de armazenamento usada da unidade.                                     |

## <a name="where-they-come-from"></a>De onde vêm

As `iops.*` `throughput.*` séries, e `latency.*` são coletadas do `Physical Disk` contador de desempenho definido no servidor onde a unidade está conectada, uma instância por unidade. Esses contadores são medidos por `partmgr.sys` e não incluem grande parte da pilha de software do Windows nem de saltos de rede. Eles são representativos do desempenho do hardware do dispositivo.

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
   > Os contadores são medidos em todo o intervalo, não amostras. Por exemplo, se a unidade estiver ociosa por 9 segundos, mas concluir 30 IOs em 10 de segundo, sua `physicaldisk.iops.total` será registrada como 3 Ios por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho Capture todas as atividades e seja robusto para ruído.

A `size.*` série é coletada da `MSFT_PhysicalDisk` classe no WMI, uma instância por unidade.

| Série                          | propriedade Source        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) :

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="additional-references"></a>Referências adicionais

- [Histórico de desempenho para Espaços de Armazenamento Diretos](performance-history.md)
