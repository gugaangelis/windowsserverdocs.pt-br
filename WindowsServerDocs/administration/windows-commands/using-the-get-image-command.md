---
title: Usando o comando Get-Image
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73b25fd70c1512f99b5097b2317c3f0403f3526c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363112"
---
# <a name="using-the-get-image-command"></a>Usando o comando Get-Image

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre uma imagem.
## <a name="syntax"></a>Sintaxe
para imagens de inicialização:
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
para imagens de instalação:
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mídia: <Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
MediaType: {instalação &#124; de inicialização}|Especifica o tipo de imagem.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, especificar o valor da arquitetura garante que a imagem correta seja retornada.|
|[/Filename:<File name>]|se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
|\mediaGroup: <Image group name>]|Especifica o grupo de imagens que contém a imagem. Se nenhum grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo será usado. Se houver mais de um grupo de imagens no servidor, você deverá usar esse parâmetro para especificar o grupo de imagens.|
## <a name="BKMK_examples"></a>Disso
Para recuperar informações sobre uma imagem de inicialização, digite um dos seguintes:
```
wdsutil /Get-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86
wdsutil /verbose /Get-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
```
Para recuperar informações sobre uma imagem de instalação, digite um dos seguintes:
```
wdsutil /Get-Imagmedia:"Windows Vista with Officemediatype:Install
wdsutil /verbose /Get-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
#### <a name="additional-references"></a>Referências adicionais
A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-Image](using-the-add-image-command.md)
[usando o comando copy-Image](using-the-copy-image-command.md)
[usando o comando Export-Image](using-the-export-image-command.md)
[usando o comando Remove-Image](using-the-remove-image-command.md)
[usando o substituir o comando de imagem](using-the-replace-image-command.md)1[subcomando: Set-Image](subcommand-set-image.md)
