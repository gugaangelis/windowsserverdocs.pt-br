---
title: Usando o comando Get-Imageus
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6531a3b69840a0a4910b2effdd3e349b76edf2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363121"
---
# <a name="using-the-get-imagegroup-command"></a>Usando o comando Get-Imageus

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre um grupo de imagens e as imagens dentro dele.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
The Media: <Image group name>|Especifica o nome do grupo de imagens.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/detailed]|Retorna os metadados da imagem para cada imagem. Se esse parâmetro não for usado, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo.|
## <a name="BKMK_examples"></a>Disso
Para exibir informações sobre um grupo de imagens, digite:
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Para exibir informações, incluindo metadados, digite:
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando Add-imageus](using-the-add-imagegroup-command.md)
[usando o comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)
[usando o comando Remove-myimageo](using-the-remove-imagegroup-command.md)
[Subcommand: Set-Image](subcommand-set-imagegroup.md)
