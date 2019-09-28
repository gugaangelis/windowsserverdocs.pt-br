---
title: Mesclar vdisk
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7023f2a6669ea6f6801e25cbfc87c950ab95a3bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373727"
---
# <a name="merge-vdisk"></a>Mesclar vdisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Mescla um VHD (disco rígido virtual) diferencial com seu VHD pai correspondente. O VHD pai será modificado para incluir as modificações do VHD diferencial.
> [!NOTE]
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintaxe
> ```
> merge vdisk depth=<n>
> ```
> ### <a name="parameters"></a>Parâmetros
> 
> | Parâmetro |                                                                                    Descrição                                                                                    |
> |-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | profundidade = <n> | Indica o número de arquivos VHD pai a serem mesclados. Por exemplo, **depth = 1** indica que o VHD diferencial será mesclado com um nível da cadeia diferencial. |
> 
> ## <a name="remarks"></a>Comentários
> - Um VHD deve ser selecionado e desanexado para que essa operação tenha sucesso. Use o comando **Select VDISK** para selecionar um VHD e deslocar o foco para ele.
> - Esse parâmetro modifica o VHD pai. Como resultado, outros VHDs diferenciais que dependem do pai não serão mais válidos.
>   ## <a name="BKMK_Examples"></a>Disso
>   Para mesclar um VHD diferencial com seu VHD pai, digite:
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>Referências adicionais
> - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
> - [anexar vdisk](attach-vdisk.md)
> - [Compact vdisk](compact-vdisk.md)

-   [detalhes do VDISK](detail-vdisk.md)
-   [Desanexar vdisk](detach-vdisk.md)
-   [expandir vdisk](expand-vdisk.md)
-   [selecionar vdisk](select-vdisk.md)
-   [list_1](list_1.md)
