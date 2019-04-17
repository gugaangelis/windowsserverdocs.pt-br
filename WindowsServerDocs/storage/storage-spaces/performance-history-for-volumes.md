---
title: Histórico de desempenho de volumes
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589697"
---
# <a name="performance-history-for-volumes"></a>Histórico de desempenho de volumes

> Aplicável à: Demonstração de Insider Windows Server

Este tópico sub-recursos do [histórico de desempenho para espaços de armazenamento direto](performance-history.md) descreve em detalhes o histórico de desempenho coletado para volumes. Histórico de desempenho está disponível para cada Cluster compartilhados Volume (CSV) no cluster. No entanto, ele não está disponível para o sistema operacional inicialize volumes nem qualquer outro armazenamento de não-CSV.

   > [!NOTE]
   > Pode levar alguns minutos para que a coleção começar a volumes recém-criado ou renomeados.

## <a name="series-names-and-units"></a>Unidades e nomes de série

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
| `volume.size.total`       |  bytes            |
| `volume.size.available`   |  bytes            |

## <a name="how-to-interpret"></a>Como interpretar

| Série                    | Como interpretar                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Número de operações de leitura por segundo concluídas por esse volume.                |
| `volume.iops.write`       | Número de operações de gravação por segundo concluídas por esse volume.               |
| `volume.iops.total`       | Número total de ler ou gravar operações por segundo concluídas por esse volume. |
| `volume.throughput.read`  | Quantidade de dados lidos deste volume por segundo.                            |
| `volume.throughput.write` | Quantidade de dados gravados esse volume por segundo.                           |
| `volume.throughput.total` | Quantidade total de dados lido ou gravado para esse volume por segundo.        |
| `volume.latency.read`     | Latência média das operações de leitura deste volume.                          |
| `volume.latency.write`    | Latência média das operações de gravação para esse volume.                           |
| `volume.latency.average`  | Latência média de todas as operações de ou para esse volume.                     |
| `volume.size.total`       | A capacidade de armazenamento total do volume.                                     |
| `volume.size.available`   | A capacidade de armazenamento disponível do volume.                                 |

## <a name="where-they-come-from"></a>Onde eles vêm

O `iops.*`, `throughput.*`, e `latency.*` série será coletada a partir do `Cluster CSVFS` conjunto de contadores de desempenho. Cada servidor do cluster possui uma instância de cada volume CSV, independentemente da propriedade. O histórico de desempenho gravadas para volume `MyVolume` é a agregação do `MyVolume` instâncias em cada servidor do cluster.

| Série                    | Contador de origem         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *soma das perguntas acima*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *soma das perguntas acima*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *Média das perguntas acima* |

   > [!NOTE]
   > Contadores são medidos durante o intervalo inteiro, não de amostra. Por exemplo, se o volume estiver ocioso por 9 segundos mas conclui 30 IOs na segunda 10º, seu `volume.iops.total` serão registradas como 3 IOs por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura todas as atividades e é robusto para ruído.

   > [!TIP]
   > Esses são os contadores mesmos usados pela estrutura de parâmetro de comparação de [VM Almirante](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) popular.

O `size.*` série será coletada a partir do `MSFT_Volume` classe WMI, uma instância por volume.

| Série                    | Propriedade Source |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-Volume](https://docs.microsoft.com/powershell/module/storage/get-volume) :

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para espaços de armazenamento direto](performance-history.md)
