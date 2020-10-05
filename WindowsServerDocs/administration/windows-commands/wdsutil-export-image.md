---
title: WDSUTIL exportar-imagem
description: Artigo de referência para WDSUTIL Export-Image, que exporta uma imagem existente do repositório de imagens para outro arquivo de imagem do Windows (. wim).
ms.topic: reference
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: cf4fd75d0e88a492b77ce6cd108d54c057503aec
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729673"
---
# <a name="wdsutil-export-image"></a>WDSUTIL exportar-imagem

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exporta uma imagem existente do repositório de imagens para outro arquivo de imagem do Windows (. wim).

## <a name="syntax"></a>Syntax
para imagens de inicialização:
```
wdsutil [Options] /Export-Image image:<Image name> [/Server:<Server name>]
    imagetype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No}]
```
para imagens de instalação:
```
wdsutil [Options] /Export-Image image:<Image name> [/Server:<Server name>]
    imagetype:Install imageGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No | append}]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
| imagem<Image name>|Especifica o nome da imagem a ser exportada.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
| ImageType: {inicialização &#124; instalação}|Especifica o tipo de imagem a ser exportada.|
|\imageGroup: <Image group name> ]|Especifica o grupo de imagens que contém a imagem a ser exportada. Se nenhum nome de grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo de imagens será usado por padrão. Se houver mais de um grupo de imagens no servidor, o grupo de imagens deverá ser especificado.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem a ser exportada. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, especificar o valor da arquitetura garante que a imagem correta será retornada.|
|[/Filename:<Filename>]|se a imagem não puder ser identificada exclusivamente pelo nome, o nome do arquivo deverá ser especificado.|
|/DestinationImage|Especifica as configurações para a imagem de destino. Você pode especificar essas configurações usando as seguintes opções:<p>-/FilePath.: <File path and name> -especifica o caminho de arquivo completo para a nova imagem.<br />-[/Name: <Name> ] – define o nome de exibição da imagem. Se nenhum nome for especificado, o nome de exibição da imagem de origem será usado.<br />- [/Description: <Description>] – Define a descrição da imagem.|
|[/Overwrite: {Yes &#124; no &#124; Append}]|Determina se o arquivo especificado na opção **/DestinationImage** será substituído se já existir um arquivo existente com esse nome no/FilePath.<p>-   **Sim** faz com que o arquivo existente seja substituído.<br />-   **Não** (a opção padrão) faz com que ocorra um erro se já existir um arquivo com o mesmo nome.<br />-   **Append** faz com que a imagem gerada seja acrescentada como uma nova imagem dentro do arquivo. wim existente.|
## <a name="examples"></a>Exemplos
Para exportar uma imagem de inicialização, digite uma das seguintes opções:
```
wdsutil /Export-Image image:WinPE boot image imagetype:Boot /Architecture:x86 /DestinationImage /Filepath:C:\temp\boot.wim
wdsutil /verbose /Progress /Export-Image image:WinPE boot image /Server:MyWDSServer imagetype:Boot /Architecture:x64 /Filename:boot.wim
/DestinationImage /Filepath:\\Server\Share\ExportImage.wim /Name:Exported WinPE image /Description:WinPE Image from WDS server /Overwrite:Yes
```
Para exportar uma imagem de instalação, digite uma das seguintes opções:
```
wdsutil /Export-Image image:Windows Vista with Office imagetype:Install /DestinationImage /Filepath:C:\Temp\Install.wim
wdsutil /verbose /Progress /Export-Image image:Windows Vista with Office /Server:MyWDSServer imagetype:Instal imageGroup:ImageGroup1
/Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:Exported Windows image /Description:Windows Vista image from WDS server /Overwrite:append
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando de adição de imagem do WDSUTIL](wdsutil-add-image.md)
- [comando de cópia de imagem do WDSUTIL](wdsutil-copy-image.md)
- [comando de Get-Image do WDSUTIL](wdsutil-get-image.md)
- [comando de remoção de imagem do WDSUTIL](wdsutil-remove-image.md)
- [comando de substituição do WDSUTIL-imagem](wdsutil-replace-image.md)
- [WDSUTIL Set-Command Image](wdsutil-set-image.md)
