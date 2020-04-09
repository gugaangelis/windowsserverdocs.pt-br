---
title: Histórico de desempenho para clusters
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7a5eec986d6e7d633f1917c599ab6fcd244c7008
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856199"
---
# <a name="performance-history-for-clusters"></a>Histórico de desempenho para clusters

> Aplica-se a: Windows Server 2019

Este subtópico do [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve o histórico de desempenho coletado para clusters.

Não há séries que se originam no nível do cluster. Em vez disso, as séries de servidores, como `clusternode.cpu.usage`, são agregadas para todos os servidores no cluster. As séries de volume, como `volume.iops.total`, são agregadas para todos os volumes no cluster. E a série de unidades, como `physicaldisk.size.total`, são agregadas para todas as unidades no cluster.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Consulte também

- [Histórico de desempenho para Espaços de Armazenamento Diretos](performance-history.md)
