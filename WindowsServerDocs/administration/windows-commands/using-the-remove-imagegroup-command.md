---
title: remover-MyImage
description: Tópico de comandos do Windows para Remove-Group-Image, que remove um grupo de imagens de um servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8406959037a958ea6d61b8e8145317635955e6d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830349"
---
# <a name="using-the-remove-imagegroup-command"></a>Usando o comando Remove-MyImage

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um grupo de imagens de um servidor.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
<Image group name> de mídia:|Especifica o nome do grupo de imagens a ser removido|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para remover o grupo de imagens, digite um dos seguintes:
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:My Image Group /Server:MyWDSServer 
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
[Usando o comando Add-Imageus](using-the-add-imagegroup-command.md)  
[Usando o comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)  
[Usando o comando Get-Imageus](using-the-get-imagegroup-command.md)  
[Subcomando: Set-grupo de imagens](subcommand-set-imagegroup.md)  
