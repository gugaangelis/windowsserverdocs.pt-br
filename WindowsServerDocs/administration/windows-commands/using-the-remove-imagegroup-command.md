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
ms.openlocfilehash: 30e4c2c7c5cf2668d62e96d8d2a54dc33e3d2a55
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960946"
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
#### <a name="additional-references"></a>referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
[Usando o comando Add-Imageus](using-the-add-imagegroup-command.md)  
[Usando o comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)  
[Usando o comando Get-Imageus](using-the-get-imagegroup-command.md)  
[Subcomando: Set-grupo de imagens](subcommand-set-imagegroup.md)  
