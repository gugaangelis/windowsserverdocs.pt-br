---
title: expandir vdisk
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c2380045de45397888777f58e3420c75bb6915ae
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725689"
---
# <a name="expand-vdisk"></a>expandir vdisk

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

expande um VHD (disco rígido virtual) para o tamanho que você especificar.
> [!NOTE]
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintaxe
> ```
> expand vdisk maximum=<n>
> ```
> ### <a name="parameters"></a>Parâmetros
> 
> |  Parâmetro  |                      Descrição                      |
> |-------------|-------------------------------------------------------|
> | máximo =<n> | Especifica o novo tamanho para o VHD em megabytes (MB). |
> 
> ## <a name="remarks"></a>Comentários
> - Um VHD deve ser selecionado e desanexado para que essa operação tenha sucesso. Use o comando **Select VDISK** para selecionar um volume e deslocar o foco para ele.
>   ## <a name="examples"></a>Exemplos
>   Para expandir o VHD selecionado para 20 GB, digite:
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Referências adicionais
> - - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)
> - [Compact vdisk](compact-vdisk.md)

-   [Desanexar vdisk](detach-vdisk.md)
-   [detalhes do VDISK](detail-vdisk.md)
-   [Mesclar vdisk](merge-vdisk.md)
-   [selecionar vdisk](select-vdisk.md)
-   [list_1](list_1.md)
