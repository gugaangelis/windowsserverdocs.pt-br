---
title: Histórico de desempenho para discos rígidos virtuais
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Espaços de Armazenamento Diretos
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880397"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Histórico de desempenho para discos rígidos virtuais

> Aplica-se a: Windows Server Insider Preview

Este tópico subpropriedades de [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve detalhadamente o histórico de desempenho coletado para arquivos de disco rígido virtual (VHD). Histórico de desempenho está disponível para cada VHD anexado a uma máquina virtual em execução, em cluster. Histórico de desempenho está disponível para formatos VHD e VHDX, no entanto, ele não está disponível para arquivos VHDX compartilhados.

   > [!NOTE]
   > Pode levar vários minutos para que a coleção começar a arquivos VHD recentemente criados ou migrados.

## <a name="series-names-and-units"></a>Unidades e nomes de séries

Essas séries são coletadas para cada disco de rígido virtual qualificado:

| série                    | Unidade             |
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

| série                    | Como interpretar                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Número de operações de leitura por segundo concluída pelo disco rígido virtual.                                         |
| `vhd.iops.write`          | Número de operações de gravação por segundo concluída pelo disco rígido virtual.                                        |
| `vhd.iops.total`          | Número total de ler ou gravar operações por segundo concluída pelo disco rígido virtual.                          |
| `vhd.throughput.read`     | Quantidade de dados lidos do disco rígido virtual por segundo.                                                     |
| `vhd.throughput.write`    | Quantidade de dados gravados no disco rígido virtual por segundo.                                                    |
| `vhd.throughput.total`    | Quantidade total de dados lidos ou gravados no disco rígido virtual por segundo.                                 |
| `vhd.latency.average`     | Latência média de todas as operações de ou para o disco rígido virtual.                                              |
| `vhd.size.current`        | O tamanho atual do arquivo do que o disco rígido virtual, se expandindo dinamicamente. Se fixo, a série não é coletada. |
| `vhd.size.maximum`        | O tamanho máximo do disco rígido virtual, se expandindo dinamicamente. Se corrigido, o é o tamanho.                  |

## <a name="where-they-come-from"></a>Onde eles vêm

O `iops.*`, `throughput.*`, e `latency.*` série é coletada a partir de `Hyper-V Virtual Storage Device` conjunto de contadores de desempenho no servidor de onde a máquina virtual está em execução, uma instância por VHD ou VHDX.

| série                    | Contador de origem         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *soma dos itens acima*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *soma dos itens acima*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > Contadores são medidos ao longo de todo o intervalo, não amostrado. Por exemplo, se o VHD está inativo para 9 segundos, mas será concluído 30 IOs na segunda, 10 de seus `vhd.iops.total` serão registradas como 3 IOs por segundo em média durante esse intervalo de 10 segundos. Isso garante que seu histórico de desempenho captura todas as atividades e é robusto para o ruído.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) cmdlet:

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

- [Histórico de desempenho para espaços de armazenamento diretos](performance-history.md)
