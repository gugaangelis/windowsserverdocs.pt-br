---
title: Histórico de desempenho para clusters
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894315"
---
# <a name="performance-history-for-clusters"></a>Histórico de desempenho para clusters

> Aplicável à: Demonstração de Insider Windows Server

Este tópico sub-recursos do [histórico de desempenho para espaços de armazenamento direto](performance-history.md) descreve o histórico de desempenho coletado para clusters.

Não há nenhuma série que se originam no nível do cluster. Em vez disso, série de servidor, como `clusternode.cpu.usage`, são agregadas para todos os servidores no cluster. Série de volume, tais como `volume.iops.total`, são agregadas para todos os volumes no cluster. E a unidade de série, tais como `physicaldisk.size.total`, são agregadas para todas as unidades no cluster.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para espaços de armazenamento direto](performance-history.md)
