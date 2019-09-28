---
title: Compact vdisk
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7efd40fa4b822636eda9f4082b5f561b452d3846
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379302"
---
# <a name="compact-vdisk"></a>Compact vdisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Reduz o tamanho físico de um arquivo de disco rígido virtual (VHD) de expansão dinâmica. Esse parâmetro é útil porque a expansão dinâmica de VHDs aumenta em tamanho à medida que você adiciona arquivos, mas eles não são reduzidos de tamanho automaticamente quando você exclui arquivos.
> [!NOTE]
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintaxe
> ```
> compact vdisk
> ```
> ## <a name="remarks"></a>Comentários
> - Um VHD de expansão dinâmica deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **Select VDISK** para selecionar um VHD e deslocar o foco para ele.
> - Você só pode compactar VHDs de expansão dinâmica que são desanexados ou anexados como somente leitura.
>   ## <a name="BKMK_Examples"></a>Disso
>   Para compactar um VHD de expansão dinâmica, digite:
>   ```
>   compact vdisk
>   ```
>   ## <a name="additional-references"></a>Referências adicionais
> - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
> - [anexar vdisk](attach-vdisk.md)

-   [detalhes do VDISK](detail-vdisk.md)
-   [Desanexar vdisk](detach-vdisk.md)
-   [expandir vdisk](expand-vdisk.md)
-   [Mesclar vdisk](merge-vdisk.md)
-   [selecionar vdisk](select-vdisk.md)
-   [list_1](list_1.md)
