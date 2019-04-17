---
title: Gerenciar discos
description: Este artigo descreve como gerenciar discos
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ae96071733b961fbe65551120894c21c633db83e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="manage-disks"></a>Gerenciar discos

> **Aplicável a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico e seus subtópicos explicam o uso do Gerenciamento de Disco para gerenciar os discos em um computador, e incluem informações sobre como converter discos entre estilos de partição diferentes e como o Windows processa o status online dos novos discos.

## <a name="converting-disk-types"></a>Conversão de tipos de discos

Embora o Gerenciamento de Disco permita alterar os diversos tipos e estilos de partição do disco, algumas das operações são irreversíveis (a menos que você reformate a unidade). Você deve considerar cuidadosamente o tipo e o estilo da partição do disco mais apropriado para seu aplicativo.

A tabela a seguir mostra as opções para converter discos entre os vários estilos de partição:

| Tipo de disco | Converter em MBR  | Converter em GPT| Converter para dinâmico |
| ---- | --- | --- |--- |
| <p>MBR (Corresponder Registro de Inicialização)</p> | <p>--</p> | <p>Permitido (se não existirem volumes no disco).</p> | <p>Permitido, mas o disco pode ficar não inicializável. Consulte a observação.</p> |
| <p>GPT (Tabela de Partição de GUID)</p> | <p>Permitido (se não existirem volumes no disco).</p> | <p>--</p>  | <p>Permitido, mas o disco pode ficar não inicializável. Consulte a observação.</p> |


> [!NOTE]
> Em um cenário de inicialização múltipla, se você tiver inicializado em um sistema operacional e converter um disco MBR básico contendo um sistema operacional alternativo para um disco dinâmico, não será mais possível inicializar no sistema operacional alternativo.

## <a name="online-and-offline-status"></a>Status online e offline

O Gerenciamento de Disco exibe o status online e offline dos discos. 

No Windows, por padrão, todos os discos recém-descobertos ficam online com acesso de leitura e gravação. No Windows Server, por padrão, discos recém-descobertos ficam online com acesso de leitura e gravação a menos que estejam em um barramento compartilhado (por exemplo, SCSI, iSCSI, SAS ou Fibre Channel). Os discos em um barramento compartilhado ficam offline na primeira vez que são detectados.

Se um disco estiver offline, você deve deixá-lo online antes de poder inicializá-lo ou criar volumes nele. 

-  Deixe um disco online ou offline ao clicar no nome do disco e, em seguida, escolher a ação apropriada.

## <a name="see-also"></a>Consulte também

-   [Initialize novos discos](initialize-new-disks.md)
-   [Mover discos para outro computador](move-disks-to-another-computer.md)
-   [Converter um disco dinâmico em disco básico](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Alterar um disco de Registro Mestre de Inicialização para Tabela de Partição de GUID](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Alterar um disco de Tabela de Partição de GUID para disco de Registro Mestre de Inicialização](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Gerenciar discos rígidos virtuais](manage-virtual-hard-disks.md)