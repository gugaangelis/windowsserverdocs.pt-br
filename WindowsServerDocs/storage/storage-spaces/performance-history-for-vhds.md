---
title: Histórico de desempenho de discos rígidos virtuais
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589714"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Histórico de desempenho de discos rígidos virtuais

> Aplicável à: Demonstração de Insider Windows Server

Este tópico sub-recursos do [histórico de desempenho para espaços de armazenamento direto](performance-history.md) descreve em detalhes o histórico de desempenho coletado para arquivos do disco rígido virtual (VHD). Histórico de desempenho está disponível para cada VHD anexado a uma máquina virtual de execução, agrupada. Histórico de desempenho está disponível para formatos VHD e o VHDX, no entanto, ele não está disponível para arquivos compartilhados VHDX.

   > [!NOTE]
   > Pode levar alguns minutos para conjunto para começar para arquivos VHD recém criados ou movidos.

## <a name="series-names-and-units"></a>Unidades e nomes de série

Essas séries são coletadas para cada disco rígido virtual de elegível:

| Série                    | Unidade             |
|---------------------------|------------------|
| `vhd.iops.read`           | por segundo       |
| `vhd.iops.write`          | por segundo       |
| `vhd.iops.total`          | por segundo       |
| `vhd.throughput.read`     | bytes por segundo |
| `vhd.throughput.write`    | bytes por segundo |
| `vhd.throughput.total`    | bytes por segundo |
| `vhd.latency.average`     | segundos          |
| `vhd.size.current`        |  bytes            |
| `vhd.size.maximum`        |  bytes            |

## <a name="how-to-interpret"></a>Como interpretar

| Série                    | Como interpretar                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Número de operações de leitura por segundo concluídas pelo disco rígido virtual.                                         |
| `vhd.iops.write`          | Número de operações de gravação por segundo concluídas pelo disco rígido virtual.                                        |
| `vhd.iops.total`          | Número total de ler ou gravar operações por segundo concluídas pelo disco rígido virtual.                          |
| `vhd.throughput.read`     | Quantidade de dados lidos do disco rígido virtual por segundo.                                                     |
| `vhd.throughput.write`    | Quantidade de dados gravados no disco rígido virtual por segundo.                                                    |
| `vhd.throughput.total`    | Quantidade total de dados lido ou gravadas no disco rígido virtual por segundo.                                 |
| `vhd.latency.average`     | Latência média de todas as operações de ou para o disco rígido virtual.                                              |
| `vhd.size.current`        | O tamanho de arquivo atual no disco rígido virtual, se expandir dinamicamente. Se fixo, as séries não são coletadas. |
| `vhd.size.maximum`        | O tamanho máximo do disco rígido virtual, se expandir dinamicamente. Se fixo, o é o tamanho.                  |

## <a name="where-they-come-from"></a>Onde eles vêm

O `iops.*`, `throughput.*`, e `latency.*` série será coletada a partir do `Hyper-V Virtual Storage Device` conjunto de contadores de desempenho no servidor onde a máquina virtual está em execução, uma instância por VHD ou VHDX.

| Série                    | Contador de origem         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *soma das perguntas acima*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *soma das perguntas acima*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > Contadores são medidos durante o intervalo inteiro, não de amostra. Por exemplo, se o VHD está inativo para 9 segundos mas conclui 30 IOs na segunda 10º, seu `vhd.iops.total` serão registradas como 3 IOs por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura todas as atividades e é robusto para ruído.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) :

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

Para obter o caminho de cada VHD da máquina virtual:

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > O cmdlet Get-VHD requer um caminho de arquivo a ser fornecido. Ele não oferece suporte a enumeração.

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para espaços de armazenamento direto](performance-history.md)
