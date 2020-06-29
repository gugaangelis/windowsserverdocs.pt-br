---
title: Histórico de desempenho para clusters
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: 8ee2e85723cc2449e8cb9c42ccb7d6b761482e3a
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474863"
---
# <a name="performance-history-for-clusters"></a>Histórico de desempenho para clusters

> Aplica-se a: Windows Server 2019

Este subtópico do [histórico de desempenho para espaços de armazenamento diretos](performance-history.md) descreve o histórico de desempenho coletado para clusters.

Não há séries que se originam no nível do cluster. Em vez disso, as séries de servidores, como `clusternode.cpu.usage` , são agregadas para todos os servidores no cluster. As séries de volume, como `volume.iops.total` , são agregadas para todos os volumes no cluster. E a série de unidades, como `physicaldisk.size.total` , são agregadas para todas as unidades no cluster.

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use o cmdlet [Get-cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="additional-references"></a>Referências adicionais

- [Histórico de desempenho para Espaços de Armazenamento Diretos](performance-history.md)
