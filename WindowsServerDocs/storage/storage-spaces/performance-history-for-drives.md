---
title: Histórico de desempenho para unidades
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Espaços de Armazenamento Diretos
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879187"
---
# <a name="performance-history-for-drives"></a>Histórico de desempenho para unidades

> Aplica-se a: Windows Server Insider Preview

Este tópico subpropriedades de [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para unidades. Histórico de desempenho está disponível para cada unidade no subsistema de armazenamento de cluster, independentemente de barramento ou tipo de mídia. No entanto, ele não está disponível para unidades de inicialização do sistema operacional.

   > [!NOTE]
   > Histórico de desempenho não pode ser coletado para unidades de um servidor que está inativo. Coleção será retomada automaticamente quando o servidor de volta a funcionar.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para cada unidade qualificada:

| série                          | Unidade             |
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

| série                          | Como interpretar                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Número de operações de leitura por segundo concluídas por unidade.                |
| `physicaldisk.iops.write`       | Número de operações de gravação por segundo concluídas por unidade.               |
| `physicaldisk.iops.total`       | Número total de ler ou gravar operações por segundo concluídas por unidade. |
| `physicaldisk.throughput.read`  | Quantidade de dados lidos do disco por segundo.                            |
| `physicaldisk.throughput.write` | Quantidade de dados gravados no disco por segundo.                           |
| `physicaldisk.throughput.total` | Quantidade total de dados lidos ou gravados para a unidade por segundo.        |
| `physicaldisk.latency.read`     | Latência média das operações de leitura da unidade.                          |
| `physicaldisk.latency.write`    | Latência média das operações de gravação para a unidade.                           |
| `physicaldisk.latency.average`  | Latência média de todas as operações de ou para a unidade.                     |
| `physicaldisk.size.total`       | A capacidade de armazenamento total da unidade.                                    |
| `physicaldisk.size.used`        | A capacidade de armazenamento usado da unidade.                                     |

## <a name="where-they-come-from"></a>Onde eles vêm

O `iops.*`, `throughput.*`, e `latency.*` série é coletada a partir de `Physical Disk` contador de desempenho definido no servidor em que a unidade está conectada, uma instância por unidade. Esses contadores são medidos por `partmgr.sys` e não incluem muito da pilha de software do Windows nem qualquer saltos de rede. Eles são representativas de desempenho de hardware do dispositivo.

| série                          | Contador de origem           |
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
   > Contadores são medidos ao longo de todo o intervalo, não amostrado. Por exemplo, se a unidade estiver ociosa por 9 segundos, mas é concluída 30 IOs na segunda, 10 de seus `physicaldisk.iops.total` serão registradas como 3 IOs por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura todas as atividades e é robusto para o ruído.

O `size.*` série é coletada a partir de `MSFT_PhysicalDisk` classe no WMI, uma instância por unidade.

| série                          | Propriedade Source        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o [Get-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) cmdlet:

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para espaços de armazenamento diretos](performance-history.md)
