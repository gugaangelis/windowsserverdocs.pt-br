---
title: Estendendo volumes em Espaços de Armazenamento Diretos
ms.assetid: fa48f8f7-44e7-4a0b-b32d-d127eff470f0
ms.prod: windows-server-threshold
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 01/23/2017
ms.localizationpriority: medium
ms.openlocfilehash: 51f58ec23c55a6cb1664d800d6f4a75dae545899
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824967"
---
# <a name="extending-volumes-in-storage-spaces-direct"></a>Estendendo volumes em Espaços de Armazenamento Diretos
> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece instruções para redimensionamento de volumes em [Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md).

## <a name="prerequisites"></a>Pré-requisitos

### <a name="capacity-in-the-storage-pool"></a>Capacidade no pool de armazenamento

Antes de redimensionar um volume, veja se você tem capacidade suficiente no pool de armazenamento para acomodar a superfície nova e maior. Por exemplo, ao redimensionar um volume de espelhamento de três vias de 1 TB para 2 TB, a superfície dele aumentará de 3 TB para 6 TB. Para que o redimensionamento tenha êxito, você precisaria de pelo menos (6 - 3) = 3 TB de capacidade disponível no pool de armazenamento.

### <a name="familiarity-with-volumes-in-storage-spaces"></a>Familiaridade com volumes em Espaços de Armazenamento

Em Espaços de Armazenamento Diretos, cada volume é composto por vários objetos empilhados: o volume compartilhado clusterizado (CSV), que é um volume; a partição; o disco, que é um disco virtual; e uma ou mais camadas de armazenamento (se aplicável). Para redimensionar um volume, você precisará redimensionar vários desses objetos.

![volumes em smapi](media/resize-volumes/volumes-in-smapi.png)

Para se familiarizar com eles, tente executar **Get -** com o substantivo correspondente no PowerShell.

Por exemplo: 

```PowerShell
Get-VirtualDisk
```

Para seguir associações entre objetos na pilha, redirecione um cmdlet **Get -** para o próximo.

Por exemplo, veja aqui como ir de um disco virtual para seu volume:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-Disk | Get-Partition | Get-Volume 
```

## <a name="step-1--resize-the-virtual-disk"></a>Etapa 1 –Redimensionar o disco virtual

O disco virtual pode usar camadas de armazenamento, ou não, dependendo de como ele foi criado.

Para verificar, execute o seguinte cmdlet:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier 
```

Se o cmdlet retornar nada, o disco virtual não usa camadas de armazenamento.

### <a name="no-storage-tiers"></a>Sem camadas de armazenamento

Se o disco virtual não tiver nenhuma camada de armazenamento, você pode redimensioná-lo diretamente usando o cmdlet **Resize-VirtualDisk** cmdlet.

Forneça o novo tamanho no parâmetro **-Tamanho**.

```PowerShell
Get-VirtualDisk <FriendlyName> | Resize-VirtualDisk -Size <Size>
```

Quando você redimensionar o **VirtualDisk**, o **disco** segue automaticamente e é redimensionado também.

![Redimensionar VirtualDisk](media/resize-volumes/Resize-VirtualDisk.gif)

### <a name="with-storage-tiers"></a>Com camadas de armazenamento

Se o disco virtual usar camadas de armazenamento, você poderá redimensionar cada camada separadamente usando o cmdlet **Resize-StorageTier** cmdlet.

Obtenha os nomes das camadas armazenamento seguindo as associações do disco virtual.

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier | Select FriendlyName
```

Em seguida, para cada camada, forneça o novo tamanho no parâmetro **-Tamanho**.

```PowerShell
Get-StorageTier <FriendlyName> | Resize-StorageTier -Size <Size>
```

> [!TIP]
> Se as camadas são diferentes tipos de mídia física (como **MediaType = SSD** e **MediaType = HDD**), você precisa garantir que tenha capacidade suficiente de cada tipo de mídia no pool de armazenamento para acomodar a nova e maior superfície de cada camada.

Quando você redimensiona as **StorageTier**(s), o **VirtualDisk** e **disco** seguem automaticamente e também são redimensionados.

![Resize-StorageTier](media/resize-volumes/Resize-StorageTier.gif)

## <a name="step-2--resize-the-partition"></a>Etapa 2 - Redimensionar a partição

Em seguida, redimensione a partição usando o cmdlet **Resize-Partition**. O disco virtual deve ter duas partições: a primeira é reservada e não deve ser modificada; a que você precisa redimensionar é **PartitionNumber = 2** e **Type = Basic**.

Forneça o novo tamanho no parâmetro **-Tamanho**. É recomendável usar o tamanho máximo compatível, conforme mostrado abaixo.

```PowerShell
# Choose virtual disk
$VirtualDisk = Get-VirtualDisk <FriendlyName>

# Get its partition
$Partition = $VirtualDisk | Get-Disk | Get-Partition | Where PartitionNumber -Eq 2

# Resize to its maximum supported size 
$Partition | Resize-Partition -Size ($Partition | Get-PartitionSupportedSize).SizeMax
```

Quando você redimensiona a **Partição**, o **Volume** e o **Volume Compartilhado Clusterizado** seguem-se automaticamente e também são redimensionados.

![Redimensionar partição](media/resize-volumes/Resize-Partition.gif)

É só isso!

> [!TIP]
> Você pode verificar se o volume tem o novo tamanho executando **Get-Volume**.

## <a name="see-also"></a>Consulte também

- [Espaços de armazenamento diretos no Windows Server 2016](storage-spaces-direct-overview.md)
- [Planejando volumes em espaços de armazenamento diretos](plan-volumes.md)
- [Criação de volumes em espaços de armazenamento diretos](create-volumes.md)
