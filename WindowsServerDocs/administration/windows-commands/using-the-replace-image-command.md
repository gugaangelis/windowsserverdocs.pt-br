---
title: Usando a comando de substituir imagem
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e396ee678e22885a50c02800d77ecea1cc5ef8ba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819647"
---
# <a name="using-the-replace-image-command"></a>Usando a comando de substituir imagem

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

substitui uma imagem existente com uma nova versão da imagem.
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
Para instalar imagens:
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
mídia:<Image name>|Especifica o nome da imagem a ser substituído.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
tipo de mídia: {inicialização &#124; instalar}|Especifica o tipo de imagem a ser substituído.|
|/ Arquitetura: {x86 &#124; ia64 &#124; x64}|Especifica a arquitetura da imagem a ser substituído. Como é possível ter o mesmo nome de imagem para imagens de inicialização diferente em diferentes arquiteturas, especificando a arquitetura garante que a imagem correta é substituída.|
|[/Filename:<File name>]|Se a imagem não pode ser identificada exclusivamente pelo nome, você deve usar essa opção para especificar o nome do arquivo.|
|/replacementImage|Especifica as configurações para a imagem de substituição. Você definir essas configurações usando as seguintes opções:<br /><br />-mediaFile: <file path> -Especifica o nome e local (caminho completo) do novo arquivo. wim.<br />-[/ SourceImage: <image name>]-Especifica a imagem a ser usada se o arquivo. wim contiver várias imagens. Essa opção se aplica apenas às imagens de instalação.<br />-[/Name:<Image name>] define o nome de exibição da imagem.<br />-[/ Descrição:<Image description>]-define a descrição da imagem.|
## <a name="BKMK_examples"></a>Exemplos
Para substituir uma imagem de inicialização, digite um dos seguintes:
```
wdsutil /replace-Imagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /replacementImagmediaFile:"C:\MyFolder\Boot.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/replacementImagmediaFile:\\MyServer\Share\Boot.wim /Name:"My WinPE Image" /Description:"WinPE Image with drivers"
```
Para substituir uma imagem de instalação, digite um dos seguintes:
```
wdsutil /replace-Imagmedia:"Windows Vista Homemediatype:Install /replacementImagmediaFile:"C:\MyFolder\Install.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"Windows Vista Pro" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:Install.wim /replacementImagmediaFile:\\MyServer\Share \Install.wim /SourceImage:"Windows Vista Ultimate" /Name:"Windows Vista Desktop" /Description:"Windows Vista Ultimate with standard business applications."
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-Image](using-the-add-image-command.md)
[usando a comando de copiar imagem](using-the-copy-image-command.md)
[usando o Comando de Export-Image](using-the-export-image-command.md)
[usando o comando get-Image](using-the-get-image-command.md)
[usando a comando de substituir imagem](using-the-replace-image-command.md) 
 [ Subcomando: set-Image](subcommand-set-image.md)
