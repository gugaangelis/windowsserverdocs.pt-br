---
title: Visão geral dos espaços de armazenamento
ms.prod: windows-server
ms.author: jgerend
ms.manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: 5de8b36916624b595d8f2ac9ddb0c000e015ee65
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366056"
---
# <a name="storage-spaces-overview"></a>Visão geral dos espaços de armazenamento

Os espaços de armazenamento são uma tecnologia no Windows e no Windows Server que pode ajudar a proteger seus dados contra falhas de unidade. Ele é conceitualmente semelhante ao RAID, implementado no software. Você pode usar espaços de armazenamento para agrupar três ou mais unidades em um pool de armazenamento e, em seguida, usar a capacidade desse pool para criar espaços de armazenamento. Normalmente, eles armazenam cópias extras dos seus dados para que, se uma de suas unidades falhar, você ainda tenha uma cópia intacta dos seus dados. Se você ficar com pouca capacidade, basta adicionar mais unidades ao pool de armazenamento.

Há quatro maneiras principais de usar espaços de armazenamento:

- **Em um computador Windows** – para obter mais informações, consulte [espaços de armazenamento no Windows 10](http://windows.microsoft.com/en-us/windows-10/storage-spaces-windows-10).
- **Em um servidor autônomo com todo o armazenamento em um único servidor** -para obter mais informações, consulte [implantar espaços de armazenamento em um servidor](deploy-standalone-storage-spaces.md)autônomo.
- **Em um servidor em cluster usando espaços de armazenamento diretos com armazenamento local e conectado diretamente em cada nó de cluster** -para obter mais informações, consulte [espaços de armazenamento diretos visão geral](storage-spaces-direct-overview.md).
- **Em um servidor em cluster com um ou mais compartimentos de armazenamento SAS compartilhados que contêm todas as unidades** -para obter mais informações, consulte [visão geral de espaços de armazenamento em um cluster com SAS compartilhada](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v%3dws.11)).

