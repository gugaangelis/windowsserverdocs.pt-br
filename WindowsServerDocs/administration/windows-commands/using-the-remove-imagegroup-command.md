---
title: Usando o comando Remove-MyImage
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51d9636013fef182c4abb74ae196e08c51ff11d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362748"
---
# <a name="using-the-remove-imagegroup-command"></a>Usando o comando Remove-MyImage

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um grupo de imagens de um servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
The Media: <Image group name>|Especifica o nome do grupo de imagens a ser removido|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="BKMK_examples"></a>Disso
Para remover o grupo de imagens, digite um dos seguintes:
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:"My Image Group" /Server:MyWDSServer 
```
#### <a name="additional-references"></a>Referências adicionais
A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando Add-Image do](using-the-add-imagegroup-command.md)
[usando o comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)
[usando o comando Get-Image myCommand](using-the-get-imagegroup-command.md)
[subcomando: Set-Image](subcommand-set-imagegroup.md)
