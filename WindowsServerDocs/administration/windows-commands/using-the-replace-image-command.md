---
title: Usando o comando Replace-Image
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68ded3df-e309-420f-9f5d-caeb609385a5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52ad932cbdaca2c708add1a52677b5d7e84d9ed6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362730"
---
# <a name="using-the-replace-image-command"></a>Usando o comando Replace-Image

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

substitui uma imagem existente por uma nova versão da imagem.
## <a name="syntax"></a>Sintaxe
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
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mídia: <Image name>|Especifica o nome da imagem a ser substituída.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
MediaType: {instalação &#124; de inicialização}|Especifica o tipo de imagem a ser substituído.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem a ser substituída. Como é possível ter o mesmo nome de imagem para diferentes imagens de inicialização em diferentes arquiteturas, especificar a arquitetura garante que a imagem correta seja substituída.|
|[/Filename:<File name>]|se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
|/replacementImage|Especifica as configurações para a imagem de substituição. Defina essas configurações usando as seguintes opções:<br /><br />-mediafile: <file path>-especifica o nome e o local (caminho completo) do novo arquivo. wim.<br />-[/SourceImage: <image name>]-Especifica a imagem a ser usada se o arquivo. wim contiver várias imagens. Essa opção se aplica somente a imagens de instalação.<br />-[/Name: <Image name>] define o nome de exibição da imagem.<br />-[/Description: <Image description>] – define a descrição da imagem.|
## <a name="BKMK_examples"></a>Disso
Para substituir uma imagem de inicialização, digite uma das seguintes opções:
```
wdsutil /replace-Imagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /replacementImagmediaFile:"C:\MyFolder\Boot.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/replacementImagmediaFile:\\MyServer\Share\Boot.wim /Name:"My WinPE Image" /Description:"WinPE Image with drivers"
```
Para substituir uma imagem de instalação, digite uma das seguintes opções:
```
wdsutil /replace-Imagmedia:"Windows Vista Homemediatype:Install /replacementImagmediaFile:"C:\MyFolder\Install.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"Windows Vista Pro" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:Install.wim /replacementImagmediaFile:\\MyServer\Share \Install.wim /SourceImage:"Windows Vista Ultimate" /Name:"Windows Vista Desktop" /Description:"Windows Vista Ultimate with standard business applications."
```
#### <a name="additional-references"></a>Referências adicionais
A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-Image](using-the-add-image-command.md)
[usando o comando copy-Image](using-the-copy-image-command.md)
[usando o comando Export-Image](using-the-export-image-command.md)
[usando o comando Get-Image](using-the-get-image-command.md)
[usando o substituir o comando de imagem](using-the-replace-image-command.md)1[subcomando: Set-Image](subcommand-set-image.md)
