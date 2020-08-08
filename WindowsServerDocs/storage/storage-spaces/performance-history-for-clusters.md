---
title: Histórico de desempenho para clusters
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: ea80be97e3940850f9892c50c534c3449fd3ecdb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954663"
---
# <a name="performance-history-for-clusters"></a>Histórico de desempenho para clusters

> Aplica-se a: Windows Server 2019

Este subtópico do [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve o histórico de desempenho coletado para clusters.

Não há séries que se originam no nível do cluster. Em vez disso, as séries de servidores, como `clusternode.cpu.usage` , são agregadas para todos os servidores no cluster. As séries de volume, como `volume.iops.total` , são agregadas para todos os volumes no cluster. E a série de unidades, como `physicaldisk.size.total` , são agregadas para todas as unidades no cluster.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-cluster](/powershell/module/failoverclusters/get-cluster) :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="additional-references"></a>Referências adicionais

- [Histórico de desempenho para Espaços de Armazenamento Diretos](performance-history.md)
