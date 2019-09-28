---
title: Usando o comando Get-AllImageGroups
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54e302dca5014d084c7277154eb491f9e33a536b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363315"
---
# <a name="using-the-get-allimagegroups-command"></a>Usando o comando Get-AllImageGroups

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre todos os grupos de imagens em um servidor e todas as imagens nesses grupos de imagens.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/detailed]|Retorna os metadados da imagem de cada imagem. Se esse parâmetro não for usado, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo para cada imagem.|
## <a name="BKMK_examples"></a>Disso
Para exibir informações sobre os grupos de imagens, digite um dos seguintes:
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando Add-imageus](using-the-add-imagegroup-command.md)
[usando o comando Get-image do grupo de imagens](using-the-get-imagegroup-command.md)
[usando o comando Remove-Image](using-the-remove-imagegroup-command.md)do 
[subcomando: Set-Image](subcommand-set-imagegroup.md)
