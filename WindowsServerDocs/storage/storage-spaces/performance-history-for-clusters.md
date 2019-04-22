---
title: Histórico de desempenho para clusters
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Espaços de Armazenamento Diretos
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818767"
---
# <a name="performance-history-for-clusters"></a>Histórico de desempenho para clusters

> Aplica-se a: Windows Server Insider Preview

Este tópico subpropriedades de [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve o histórico de desempenho coletado para clusters.

Não há nenhuma série que se originam no nível do cluster. Em vez disso, a série de servidor, como `clusternode.cpu.usage`, são agregados para todos os servidores no cluster. Série do volume, como `volume.iops.total`, são agregados para todos os volumes no cluster. E a unidade de série, tais como `physicaldisk.size.total`, são agregados para todas as unidades no cluster.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o [Get-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) cmdlet:

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para espaços de armazenamento diretos](performance-history.md)
