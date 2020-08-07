---
title: Histórico de desempenho para discos rígidos virtuais
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 458c9578e541d67135836557af7789c671ed001d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935675"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Histórico de desempenho para discos rígidos virtuais

> Aplica-se a: Windows Server 2019

Este subtópico do [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para arquivos de disco rígido virtual (VHD). O histórico de desempenho está disponível para cada VHD anexado a uma máquina virtual clusterizada em execução. O histórico de desempenho está disponível para os formatos VHD e VHDX, no entanto, ele não está disponível para arquivos VHDX compartilhados.

   > [!NOTE]
   > Pode levar vários minutos para que a coleta comece para arquivos VHD recém-criados ou movidos.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para cada disco rígido virtual qualificado:

| Série                    | Unidade             |
|---------------------------|------------------|
| `vhd.iops.read`           | por segundo       |
| `vhd.iops.write`          | por segundo       |
| `vhd.iops.total`          | por segundo       |
| `vhd.throughput.read`     | bytes por segundo |
| `vhd.throughput.write`    | bytes por segundo |
| `vhd.throughput.total`    | bytes por segundo |
| `vhd.latency.average`     | segundos          |
| `vhd.size.current`        | bytes            |
| `vhd.size.maximum`        | bytes            |

## <a name="how-to-interpret"></a>Como interpretar

| Série                    | Como interpretar                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Número de operações de leitura por segundo concluídas pelo disco rígido virtual.                                         |
| `vhd.iops.write`          | Número de operações de gravação por segundo concluídas pelo disco rígido virtual.                                        |
| `vhd.iops.total`          | Número total de operações de leitura ou gravação por segundo concluídas pelo disco rígido virtual.                          |
| `vhd.throughput.read`     | Quantidade de dados lidos do disco rígido virtual por segundo.                                                     |
| `vhd.throughput.write`    | Quantidade de dados gravados no disco rígido virtual por segundo.                                                    |
| `vhd.throughput.total`    | Quantidade total de dados lidos ou gravados no disco rígido virtual por segundo.                                 |
| `vhd.latency.average`     | Latência média de todas as operações de ou para o disco rígido virtual.                                              |
| `vhd.size.current`        | O tamanho de arquivo atual do disco rígido virtual, se expandindo dinamicamente. Se for corrigido, a série não será coletada. |
| `vhd.size.maximum`        | O tamanho máximo do disco rígido virtual, se expandindo dinamicamente. Se fixo, o é o tamanho.                  |

## <a name="where-they-come-from"></a>De onde vêm

As `iops.*` `throughput.*` séries, e `latency.*` são coletadas do `Hyper-V Virtual Storage Device` contador de desempenho definido no servidor em que a máquina virtual está em execução, uma instância por VHD ou VHDX.

| Série                    | Contador de origem         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *soma dos itens acima*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *soma dos itens acima*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > Os contadores são medidos em todo o intervalo, não amostras. Por exemplo, se o VHD estiver inativo por 9 segundos, mas concluir 30 IOs no décimo segundo, seu `vhd.iops.total` será registrado como 3 Ios por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho Capture todas as atividades e seja robusto para ruído.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-VHD](/powershell/module/hyper-v/get-vhd) :

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

Para obter o caminho de cada VHD da máquina virtual:

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > O cmdlet Get-VHD requer que um caminho de arquivo seja fornecido. Ele não oferece suporte à enumeração.

## <a name="additional-references"></a>Referências adicionais

- [Histórico de desempenho para Espaços de Armazenamento Diretos](performance-history.md)
