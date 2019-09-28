---
title: Estendendo volumes em Espaços de Armazenamento Diretos
description: Como redimensionar volumes em Espaços de Armazenamento Diretos usando o centro de administração do Windows e o PowerShell.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 05/07/2019
ms.openlocfilehash: 20482fe1728b12d4fe56dcfa397352fbb4b4f981
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366087"
---
# <a name="extending-volumes-in-storage-spaces-direct"></a>Estendendo volumes em Espaços de Armazenamento Diretos
> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece instruções para redimensionar volumes em um cluster [espaços de armazenamento diretos](storage-spaces-direct-overview.md) usando o centro de administração do Windows.

Assista a um vídeo rápido sobre como redimensionar um volume.

> [!VIDEO https://www.youtube-nocookie.com/embed/hqyBzipBoTI]

## <a name="extending-volumes-using-windows-admin-center"></a>Estendendo volumes usando o centro de administração do Windows

1. No centro de administração do Windows, conecte-se a um cluster Espaços de Armazenamento Diretos e, em seguida, selecione **volumes** no painel **ferramentas** .
2. Na página volumes, selecione a guia **inventário** e, em seguida, selecione o volume que você deseja redimensionar.

    Na página detalhes do volume, a capacidade de armazenamento para o volume é indicada. Você também pode abrir a página de detalhes de volumes diretamente do painel. No painel de alertas, selecione o alerta, que notifica se um volume está ficando com pouca capacidade de armazenamento e, em seguida, selecione **ir para volume**.

4. Na parte superior da página de detalhes de volumes, selecione **redimensionar**.
5. Insira um novo tamanho maior e, em seguida, selecione **redimensionar**.

    Na página de detalhes volumes, a capacidade de armazenamento maior para o volume é indicada e o alerta no painel é limpo.

## <a name="extending-volumes-using-powershell"></a>Estendendo volumes usando o PowerShell

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

### <a name="step-1--resize-the-virtual-disk"></a>Etapa 1 –Redimensionar o disco virtual

O disco virtual pode usar camadas de armazenamento, ou não, dependendo de como ele foi criado.

Para verificar, execute o seguinte cmdlet:

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier 
```

Se o cmdlet retornar nada, o disco virtual não usa camadas de armazenamento.

#### <a name="no-storage-tiers"></a>Sem camadas de armazenamento

Se o disco virtual não tiver nenhuma camada de armazenamento, você pode redimensioná-lo diretamente usando o cmdlet **Resize-VirtualDisk** cmdlet.

Forneça o novo tamanho no parâmetro **-Tamanho**.

```PowerShell
Get-VirtualDisk <FriendlyName> | Resize-VirtualDisk -Size <Size>
```

Quando você redimensionar o **VirtualDisk**, o **disco** segue automaticamente e é redimensionado também.

![Redimensionar VirtualDisk](media/resize-volumes/Resize-VirtualDisk.gif)

#### <a name="with-storage-tiers"></a>Com camadas de armazenamento

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

### <a name="step-2--resize-the-partition"></a>Etapa 2 - Redimensionar a partição

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

- [Espaços de Armazenamento Diretos no Windows Server 2016](storage-spaces-direct-overview.md)
- [Planejando volumes no Espaços de Armazenamento Diretos](plan-volumes.md)
- [Criando volumes no Espaços de Armazenamento Diretos](create-volumes.md)
- [Excluindo volumes no Espaços de Armazenamento Diretos](delete-volumes.md)
