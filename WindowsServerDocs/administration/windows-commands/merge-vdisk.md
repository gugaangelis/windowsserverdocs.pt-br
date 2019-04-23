---
title: Mesclar vdisk
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 159e307912cb06bc3ea5e452f735e786c0cf9965
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847007"
---
# <a name="merge-vdisk"></a>Mesclar vdisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Mescla um disco rígido virtual diferencial (VHD) com o VHD pai correspondente. O VHD pai será modificado para incluir as modificações do VHD diferencial.
> [!NOTE]
> Este comando só é aplicável ao Windows 7 e Windows Server 2008 R2.
## <a name="syntax"></a>Sintaxe
```
merge vdisk depth=<n>
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|depth=<n>|Indica o número de arquivos do VHD pai juntos de mesclagem. Por exemplo, **profundidade = 1** indica que o VHD diferencial será mesclado com um nível da cadeia de diferenciação.|
## <a name="remarks"></a>Comentários
-   Um VHD deve ser selecionado e desanexado para essa operação seja bem-sucedida. Use o **selecionar o vdisk** comando para selecionar um VHD e mudar o foco a ele.
-   Esse parâmetro modifica o VHD pai. Como resultado, os outros VHDs diferenciais que dependem do pai não serão válidos.
## <a name="BKMK_Examples"></a>Exemplos
Para mesclar um VHD diferencial com seu VHD pai, digite:
```
merge vdisk depth=1
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [attach vdisk](attach-vdisk.md)
-   [Compactar vdisk](compact-vdisk.md)

-   [Detalhar vdisk](detail-vdisk.md)
-   [Desanexar vdisk](detach-vdisk.md)
-   [Expandir vdisk](expand-vdisk.md)
-   [Selecionar o vdisk](select-vdisk.md)
-   [list_1](list_1.md)
