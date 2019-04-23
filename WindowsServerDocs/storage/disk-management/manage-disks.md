---
title: Gerenciar discos
description: Este artigo descreve como gerenciar discos
ms.date: 12/21/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f4698dac683ff3769eb4403ae2750ad38a301022
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846187"
---
# <a name="manage-disks"></a>Gerenciar discos

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico e seus subtópicos abordam o uso do gerenciamento de disco para gerenciar os discos em um computador e inclui informações sobre a inicialização de novos discos, converter discos entre os estilos de partição diferentes, e como o Windows lida com o status online de novos discos.

## <a name="online-and-offline-status"></a>Status online e offline

Gerenciamento de disco exibe se um disco está online (disponível) ou off-line.

No Windows, por padrão, todos os discos recém-descobertos ficam online com acesso de leitura e gravação. No Windows Server, por padrão, discos recém-descobertos ficam online com acesso de leitura e gravação a menos que estejam em um barramento compartilhado (por exemplo, SCSI, iSCSI, SAS ou Fibre Channel). Discos em um barramento compartilhado estão offline na primeira vez em que elas forem detectadas.

Se um disco estiver offline, você deve deixá-lo online antes de poder inicializá-lo ou criar volumes nele.

Para colocar um disco online ou offline, clique com botão direito no nome do disco e, em seguida, escolhendo a ação apropriada.





## <a name="see-also"></a>Consulte também

-   [Inicializar novos discos](initialize-new-disks.md)
-   [Mover discos para outro computador](move-disks-to-another-computer.md)
-   [Alterar um disco dinâmico em um disco básico](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Converter um disco de registro mestre de inicialização em um disco de tabela de partição GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Converter um disco de tabela de partição GUID em um disco de registro mestre de inicialização](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Gerenciar discos rígidos virtuais](manage-virtual-hard-disks.md)