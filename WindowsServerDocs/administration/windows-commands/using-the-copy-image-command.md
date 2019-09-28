---
title: Usando o comando copy-Image
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f269884e9c1f96151c9e1220242fad917b76831
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363560"
---
# <a name="using-the-copy-image-command"></a>Usando o comando copy-Image

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia imagens que estão dentro do mesmo grupo de imagens. Para copiar imagens entre grupos de imagens, use o comando de [comando Export-Image](using-the-export-image-command.md) e, em seguida, o [usando o comando de comando Add-Image](using-the-add-image-command.md) .
para obter exemplos de como você pode usar esse comando, consulte [exemplos](#BKMK_examples).
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /copy-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Name:<Name>
         /Filename:<File name>
         [/Description:<Description>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mídia: <Image name>|Especifica o nome da imagem a ser copiada.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
MediaType: instalar|Especifica o tipo de imagem a ser copiada. Essa opção deve ser definida como **instalar**.|
|\mediaGroup: <Image group name>]|Especifica o grupo de imagens que contém a imagem a ser copiada. Se nenhum grupo de imagens for especificado e houver apenas um grupo no servidor, esse grupo de imagens será usado por padrão. Se houver mais de um grupo de imagens no servidor, você deverá especificar o grupo de imagens.|
|[/Filename:<Filename>]|Especifica o nome do arquivo da imagem a ser copiada. Se a imagem de origem não puder ser identificada exclusivamente pelo nome, você deverá especificar o nome do arquivo.|
|/DestinationImage|Especifica as configurações para a imagem de destino, conforme descrito na tabela a seguir.<br /><br />-/Name: <Name>-define o nome de exibição da imagem a ser copiada.<br />-/Filename: <Filename>-define o nome do arquivo de imagem de destino que conterá a cópia da imagem.<br />- [/Description: <Description>] – Define a descrição da cópia da imagem.|
## <a name="BKMK_examples"></a>Disso
Para criar uma cópia da imagem especificada e nomeá-la Aplicativoscompatíveis. wim, digite:
```
wdsutil /copy-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim"
```
Para criar uma cópia da imagem especificada, aplique as configurações especificadas e nomeie a cópia Aplicativoscompatíveis. wim, digite:
```
wdsutil /verbose /Progress /copy-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim" /Description:"This is a copy of the original Windows image with Office installed"
```
#### <a name="additional-references"></a>Referências adicionais
A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-Image](using-the-add-image-command.md)
[usando o comando Export-Image](using-the-export-image-command.md)
[usando o comando Get-Image](using-the-get-image-command.md)
[usando o comando Remove-Image](using-the-remove-image-command.md)
[usando o substituir o comando de imagem](using-the-replace-image-command.md)1[subcomando: Set-Image](subcommand-set-image.md)
