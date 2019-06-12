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
ms.openlocfilehash: 8019ce62d6cf38c7430a789f68749444ac91ad48
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439445"
---
# <a name="expand-vdisk"></a>Expandir vdisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

expande um disco rígido virtual (VHD) para o tamanho que você especifica.
> [!NOTE]
> Este comando só é aplicável ao Windows 7 e Windows Server 2008 R2.
> ## <a name="syntax"></a>Sintaxe
> ```
> expand vdisk maximum=<n>
> ```
> ## <a name="parameters"></a>Parâmetros
> 
> |  Parâmetro  |                      Descrição                      |
> |-------------|-------------------------------------------------------|
> | maximum=<n> | Especifica o novo tamanho para o VHD em megabytes (MB). |
> 
> ## <a name="remarks"></a>Comentários
> - Um VHD deve ser selecionado e desanexado para essa operação seja bem-sucedida. Use o **selecionar o vdisk** comando para selecionar um volume e mudar o foco a ele.
>   ## <a name="BKMK_Examples"></a>Exemplos
>   Para expandir o VHD selecionado para 20 GB, digite:
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Referências adicionais
> - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)
> - [Compactar vdisk](compact-vdisk.md)

-   [Desanexar vdisk](detach-vdisk.md)
-   [Detalhar vdisk](detail-vdisk.md)
-   [Mesclar vdisk](merge-vdisk.md)
-   [Selecionar o vdisk](select-vdisk.md)
-   [list_1](list_1.md)
