---
title: Substituir imagem
description: Artigo de referência para Replace-Image, que substitui uma imagem existente por uma nova versão dessa imagem.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68ded3df-e309-420f-9f5d-caeb609385a5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b98bf14b944ce75a21efbbb38a211e60ca952d39
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931351"
---
# <a name="using-the-replace-image-command"></a>Usando o comando Replace-Image

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Substitui uma imagem existente por uma nova versão da imagem.
## <a name="syntax"></a>Syntax
para imagens de inicialização:
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/Name:<Image name>]
         [/Description:<Image description>]
```
para imagens de instalação:
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/SourceImage:<Source image name>]
         [/Name:<Image name>]
         [/Description:<Image description>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
meio<Image name>|Especifica o nome da imagem a ser substituída.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
MediaType: {boot &#124; install}|Especifica o tipo de imagem a ser substituído.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem a ser substituída. Como é possível ter o mesmo nome de imagem para diferentes imagens de inicialização em diferentes arquiteturas, especificar a arquitetura garante que a imagem correta seja substituída.|
|[/Filename:<File name>]|se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
|/replacementImage|Especifica as configurações para a imagem de substituição. Defina essas configurações usando as seguintes opções:<p>-mediafile: <file path> -especifica o nome e o local (caminho completo) do novo arquivo. wim.<br />-[/SourceImage: <image name> ]-Especifica a imagem a ser usada se o arquivo. wim contiver várias imagens. Essa opção se aplica somente a imagens de instalação.<br />-[/Name: <Image name> ] define o nome de exibição da imagem.<br />-[/Description: <Image description> ] – define a descrição da imagem.|
## <a name="examples"></a>Exemplos
Para substituir uma imagem de inicialização, digite uma das seguintes opções:
```
wdsutil /replace-Imagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /replacementImagmediaFile:C:\MyFolder\Boot.wim
wdsutil /verbose /Progress /replace-Imagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim
/replacementImagmediaFile:\\MyServer\Share\Boot.wim /Name:My WinPE Image /Description:WinPE Image with drivers
```
Para substituir uma imagem de instalação, digite uma das seguintes opções:
```
wdsutil /replace-Imagmedia:Windows Vista Homemediatype:Install /replacementImagmediaFile:C:\MyFolder\Install.wim
wdsutil /verbose /Progress /replace-Imagmedia:Windows Vista Pro /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1
/Filename:Install.wim /replacementImagmediaFile:\\MyServer\Share \Install.wim /SourceImage:Windows Vista Ultimate /Name:Windows Vista Desktop /Description:Windows Vista Ultimate with standard business applications.
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-add-image-command.md) 
 Add-Image [Usando o comando](using-the-copy-image-command.md) 
 Copy-Image [Usando o comando](using-the-export-image-command.md) 
 Export-Image [Usando o comando](using-the-get-image-command.md) 
 Get-Image [Usando o comando](using-the-replace-image-command.md) 
 replace-Image [Subcomando: Set-Image](subcommand-set-image.md)
