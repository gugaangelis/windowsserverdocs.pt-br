---
title: Visão geral dos espaços de armazenamento
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: 9977bb35be3676e31cdcab7322b5b5a2cfc67609
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832907"
---
# <a name="storage-spaces-overview"></a>Visão geral dos espaços de armazenamento

Espaços de armazenamento é uma tecnologia do Windows e Windows Server que podem ajudar a proteger seus dados contra falhas de unidade. Ele é conceitualmente semelhante a RAID, implementado no software. Você pode usar espaços de armazenamento para três ou mais unidades juntas em um pool de armazenamento de grupo e, em seguida, usar capacidade desse pool para criar espaços de armazenamento. Eles geralmente armazenam cópias adicionais dos seus dados para que se uma de suas unidades falhar, você ainda terá uma cópia intacta dos seus dados. Se você executar baixa capacidade, basta adicione mais discos ao pool de armazenamento.

Há quatro formas principais para usar os espaços de armazenamento:

- **Em um PC com Windows** - para obter mais informações, consulte [espaços de armazenamento no Windows 10](http://windows.microsoft.com/en-us/windows-10/storage-spaces-windows-10).
- **Em um servidor autônomo com todo o armazenamento em um único servidor** - para obter mais informações, consulte [implantar espaços de armazenamento em um servidor autônomo](deploy-standalone-storage-spaces.md).
- **Em um servidor clusterizado usando espaços de armazenamento diretos com o armazenamento anexado direto local em cada nó de cluster** - para obter mais informações, consulte [visão geral de espaços de armazenamento diretos](storage-spaces-direct-overview.md).
- **Em um servidor em cluster com um ou mais compartilhado SAS compartimentos de armazenamento que contém todas as unidades** - para obter mais informações, consulte [espaços de armazenamento em um cluster com visão geral SAS compartilhada](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v%3dws.11)).

