---
title: Gerenciar discos
description: Este artigo descreve como gerenciar discos
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 344dd363e970b195abe20fcb69e741c450fc7a21
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812404"
---
# <a name="manage-disks"></a>Gerenciar discos

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico e seus subtópicos explicam o uso do Gerenciamento de Disco para gerenciar os discos em um computador e incluem informações sobre como inicializar discos, converter discos entre diferentes estilos de partição e como o Windows processa o status online dos novos discos.

## <a name="online-and-offline-status"></a>Status online e offline

O Gerenciamento de Disco exibe se um disco está online (disponível) ou offline.

No Windows, por padrão, todos os discos recém-descobertos ficam online com acesso de leitura e gravação. No Windows Server, por padrão, os discos recém-descobertos ficam online com acesso de leitura e gravação a menos que estejam em um barramento compartilhado (por exemplo, SCSI, iSCSI, SAS ou Fibre Channel). Os discos em um barramento compartilhado ficam offline na primeira vez que são detectados.

Se um disco estiver offline, você deve deixá-lo online antes de poder inicializá-lo ou criar volumes nele.

Para deixar um disco online ou offline, clique com o botão direito no nome do disco e, em seguida, escolha a ação apropriada.

## <a name="see-also"></a>Consulte também

-   [Inicializar novos discos](initialize-new-disks.md)
-   [Mover discos para outro computador](move-disks-to-another-computer.md)
-   [Converter um disco dinâmico em disco básico](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Transformar um disco de Registro Mestre de Inicialização em um disco de Tabela de Partição GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Transformar um disco de Tabela de Partição GUID em um disco de Registro Mestre de Inicialização](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Gerenciar discos rígidos virtuais](manage-virtual-hard-disks.md)