---
title: obter imagem
description: Artigo de referência para Get-Image, que recupera informações sobre uma imagem.
ms.topic: reference
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c3e8a25725f939c6a7a7692d192b63bac9ffd41
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029614"
---
# <a name="get-image"></a>obter imagem

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
meio<Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
MediaType: {boot &#124; install}|Especifica o tipo de imagem.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, especificar o valor da arquitetura garante que a imagem correta seja retornada.|
|[/Filename:<File name>]|se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
|\mediaGroup: <Image group name> ]|Especifica o grupo de imagens que contém a imagem. Se nenhum grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo será usado. Se houver mais de um grupo de imagens no servidor, você deverá usar esse parâmetro para especificar o grupo de imagens.|
## <a name="examples"></a>Exemplos
Para recuperar informações sobre uma imagem de inicialização, digite um dos seguintes:
```
wdsutil /Get-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86
wdsutil /verbose /Get-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
```
Para recuperar informações sobre uma imagem de instalação, digite um dos seguintes:
```
wdsutil /Get-Imagmedia:Windows Vista with Officemediatype:Install
wdsutil /verbose /Get-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-add-image-command.md) 
 Add-Image [Usando o comando](using-the-copy-image-command.md) 
 Copy-Image [Usando o comando](using-the-export-image-command.md) 
 Export-Image [Usando o comando](using-the-remove-image-command.md) 
 Remove-Image [Usando o comando](using-the-replace-image-command.md) 
 replace-Image [Subcomando: Set-Image](subcommand-set-image.md)
