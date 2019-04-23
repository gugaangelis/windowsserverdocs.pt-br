---
title: Expandir vdisk
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf8fd0b05ca5baeeee4fadd670adb3169130d04e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837397"
---
# <a name="expand-vdisk"></a>Expandir vdisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

expande um disco rígido virtual (VHD) para o tamanho que você especifica.
> [!NOTE]
> Este comando só é aplicável ao Windows 7 e Windows Server 2008 R2.
## <a name="syntax"></a>Sintaxe
```
expand vdisk maximum=<n>
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|maximum=<n>|Especifica o novo tamanho para o VHD em megabytes (MB).|
## <a name="remarks"></a>Comentários
-   Um VHD deve ser selecionado e desanexado para essa operação seja bem-sucedida. Use o **selecionar o vdisk** comando para selecionar um volume e mudar o foco a ele.
## <a name="BKMK_Examples"></a>Exemplos
Para expandir o VHD selecionado para 20 GB, digite:
```
expand vdisk maximum=20000
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [attach vdisk](attach-vdisk.md)
-   [Compactar vdisk](compact-vdisk.md)

-   [Desanexar vdisk](detach-vdisk.md)
-   [Detalhar vdisk](detail-vdisk.md)
-   [Mesclar vdisk](merge-vdisk.md)
-   [Selecionar o vdisk](select-vdisk.md)
-   [list_1](list_1.md)
