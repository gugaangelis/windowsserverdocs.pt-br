---
title: Usando o comando Export-Image
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: efa9b2d09c37a383a91883ee02c995eedb2f235e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823137"
---
# <a name="using-the-export-image-command"></a>Usando o comando Export-Image

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exporta uma imagem existente do repositório de imagens para outro arquivo de imagem do Windows (. wim).
## <a name="syntax"></a>Sintaxe
para imagens de inicialização:
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No}]
```
Para instalar imagens:
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No | append}]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mídia:<Image name>|Especifica o nome da imagem a ser exportado.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
tipo de mídia: {inicialização &#124; instalar}|Especifica o tipo de imagem a ser exportado.|
|\mediaGroup:<Image group name>]|Especifica o grupo de imagens que contém a imagem a ser exportado. Se nenhum nome de grupo de imagem for especificado e o grupo de apenas uma imagem existe no servidor, esse grupo será usado por padrão. Se houver mais de um grupo de imagens no servidor, o grupo de imagens deve ser especificado.|
|/ Arquitetura: {x86 &#124; ia64 &#124; x64}|Especifica a arquitetura da imagem a ser exportado. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, especificando o valor de arquitetura garante que a imagem correta será retornada.|
|[/Filename:<Filename>]|Se a imagem não pode ser identificada exclusivamente pelo nome, o nome do arquivo deve ser especificado.|
|/DestinationImage|Especifica as configurações para a imagem de destino. Você pode especificar essas configurações usando as seguintes opções:<br /><br />-/Filepath:<File path and name> -Especifica o caminho completo para a nova imagem.<br />-[/Name:<Name>]-define o nome de exibição da imagem. Se nenhum nome for especificado, será usado o nome de exibição da imagem de origem.<br />-[/ Descrição: <Description>]-Define a descrição da imagem.|
|[/Overwrite: {Sim &#124; não &#124; acrescentar}]|Determina se o arquivo especificado na **/DestinationImage** opção será substituída se um arquivo existente com esse nome já existe no /FilePath.<br /><br />-   **Sim** faz com que o arquivo existente seja substituído.<br />-   **Não** (a opção padrão) causa um erro ocorrerá se um arquivo com o mesmo nome já existe.<br />-   **acrescentar** faz com que a imagem gerada a ser acrescentada como uma nova imagem dentro do arquivo. wim existente.|
## <a name="BKMK_examples"></a>Exemplos
Para exportar uma imagem de inicialização, digite um dos seguintes:
```
wdsutil /Export-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /DestinationImage /Filepath:"C:\temp\boot.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/DestinationImage /Filepath:"\\Server\Share\ExportImage.wim" /Name:"Exported WinPE image" /Description:"WinPE Image from WDS server" /Overwrite:Yes
```
Para exportar uma imagem de instalação, digite o seguinte:
```
wdsutil /Export-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Filepath:"C:\Temp\Install.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:"Exported Windows image" /Description:"Windows Vista image from WDS server" /Overwrite:append
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-Image](using-the-add-image-command.md)
[usando a comando de copiar imagem](using-the-copy-image-command.md)
[usando a imagem de get Comando](using-the-get-image-command.md)
[usando o comando de remove-Image](using-the-remove-image-command.md)
[usando a comando de substituir imagem](using-the-replace-image-command.md)
[subcomando : Definir imagem](subcommand-set-image.md)
