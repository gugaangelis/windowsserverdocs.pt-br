---
title: Histórico de desempenho para volumes
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Espaços de Armazenamento Diretos
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882947"
---
# <a name="performance-history-for-volumes"></a>Histórico de desempenho para volumes

> Aplica-se a: Windows Server Insider Preview

Este tópico subpropriedades de [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para volumes. Histórico de desempenho está disponível para cada Cluster CSV (Volume compartilhado) no cluster. No entanto, ele não está disponível para o sistema operacional de inicialização volumes nem qualquer outro armazenamento não CSV.

   > [!NOTE]
   > Pode levar vários minutos para que a coleção começar a volumes recém-criados ou renomeados.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para cada volume qualificado:

| série                    | Unidade             |
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

| série                    | Como interpretar                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Número de operações de leitura por segundo concluído por este volume.                |
| `volume.iops.write`       | Número de operações de gravação por segundo concluído por este volume.               |
| `volume.iops.total`       | Número total de ler ou gravar operações por segundo concluído por este volume. |
| `volume.throughput.read`  | Quantidade de dados lidos deste volume por segundo.                            |
| `volume.throughput.write` | Quantidade de dados gravados para este volume por segundo.                           |
| `volume.throughput.total` | Quantidade total de dados lidos ou gravados para este volume por segundo.        |
| `volume.latency.read`     | Latência média das operações de leitura deste volume.                          |
| `volume.latency.write`    | Latência média das operações de gravação para este volume.                           |
| `volume.latency.average`  | Latência média de todas as operações de ou para este volume.                     |
| `volume.size.total`       | A capacidade de armazenamento total do volume.                                     |
| `volume.size.available`   | A capacidade de armazenamento disponível do volume.                                 |

## <a name="where-they-come-from"></a>Onde eles vêm

O `iops.*`, `throughput.*`, e `latency.*` série é coletada a partir de `Cluster CSVFS` conjunto de contadores de desempenho. Todos os servidores no cluster tem uma instância de cada volume CSV, independentemente da propriedade. O histórico de desempenho registrados para o volume `MyVolume` é a agregação do `MyVolume` instâncias em todos os servidores no cluster.

| série                    | Contador de origem         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *soma dos itens acima*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *soma dos itens acima*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *Média dos itens acima* |

   > [!NOTE]
   > Contadores são medidos ao longo de todo o intervalo, não amostrado. Por exemplo, se o volume estiver ocioso por 9 segundos, mas é concluída 30 IOs na segunda, 10 de seus `volume.iops.total` serão registradas como 3 IOs por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura todas as atividades e é robusto para o ruído.

   > [!TIP]
   > Esses são os mesmos contadores usados por popular [frota de VM](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) estrutura de benchmark.

O `size.*` série é coletada a partir de `MSFT_Volume` classe no WMI, uma instância por volume.

| série                    | Propriedade Source |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o [Get-Volume](https://docs.microsoft.com/powershell/module/storage/get-volume) cmdlet:

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para espaços de armazenamento diretos](performance-history.md)
