---
title: expandir vdisk
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 272714372a35f7f205b5a2e70cb2f2669b3a0634
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844889"
---
# <a name="expand-vdisk"></a>expandir vdisk

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
>   ## <a name="examples"></a><a name=BKMK_Examples></a>Disso
>   Para expandir o VHD selecionado para 20 GB, digite:
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Referências adicionais
> - - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
> - [anexar vdisk](attach-vdisk.md)
> - [Compact vdisk](compact-vdisk.md)

-   [Desanexar vdisk](detach-vdisk.md)
-   [detalhes do VDISK](detail-vdisk.md)
-   [Mesclar vdisk](merge-vdisk.md)
-   [selecionar vdisk](select-vdisk.md)
-   [list_1](list_1.md)
