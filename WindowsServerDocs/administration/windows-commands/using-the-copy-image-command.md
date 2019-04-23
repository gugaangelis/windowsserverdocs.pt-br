---
title: Usando a comando de copiar imagem
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 52c9c0bb45e60e76077bf90534e93f2c6fc1df18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884037"
---
# <a name="using-the-copy-image-command"></a>Usando a comando de copiar imagem

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imagens de cópias que estão dentro do mesmo grupo de imagem. Para copiar imagens entre grupos de imagens, use o [usando o comando Export-Image](using-the-export-image-command.md) comando e, em seguida, o [usando a imagem de adicionar comando](using-the-add-image-command.md) comando.
Para obter exemplos de como você pode usar esse comando, consulte [exemplos](#BKMK_examples).
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
mídia:<Image name>|Especifica o nome da imagem a ser copiado.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
mediatype:Install|Especifica o tipo de imagem a ser copiado. Essa opção deve ser definida como **instalar**.|
|\mediaGroup:<Image group name>]|Especifica o grupo de imagens que contém a imagem a ser copiado. Se nenhum grupo de imagem for especificado, e apenas um grupo existe no servidor, esse grupo será usado por padrão. Se houver mais de um grupo de imagens no servidor, você deverá especificar o grupo de imagens.|
|[/Filename:<Filename>]|Especifica o nome do arquivo da imagem a ser copiado. Se a imagem de origem não pode ser identificada exclusivamente pelo nome, você deve especificar o nome do arquivo.|
|/DestinationImage|Especifica as configurações para a imagem de destino, conforme descrito na tabela a seguir.<br /><br />-/Name:<Name> -define o nome de exibição da imagem a ser copiado.<br />-/Filename:<Filename> -define o nome do arquivo de imagem de destino que conterá a cópia da imagem.<br />-[/ Descrição: <Description>]-Define a descrição da cópia de imagem.|
## <a name="BKMK_examples"></a>Exemplos
Para criar uma cópia da imagem especificada e nomeie-o WindowsVista.wim, digite:
```
wdsutil /copy-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim"
```
Para criar uma cópia da imagem especificada, aplique as configurações especificadas e nomeie a cópia WindowsVista.wim, tipo:
```
wdsutil /verbose /Progress /copy-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim" /Description:"This is a copy of the original Windows image with Office installed"
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-Image](using-the-add-image-command.md)
[usando o comando Export-Image](using-the-export-image-command.md)
[usando o Imagem do comando Get-](using-the-get-image-command.md)
[usando o comando de remove-Image](using-the-remove-image-command.md)
[usando a comando de substituir imagem](using-the-replace-image-command.md) 
 [ Subcomando: set-Image](subcommand-set-image.md)
