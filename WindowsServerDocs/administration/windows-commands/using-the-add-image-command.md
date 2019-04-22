---
title: Usando a imagem de adicionar comando
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0433e0775bd2088170ae17fcfe432cdaee0bf99d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817457"
---
# <a name="using-the-add-image-command"></a>Usando a imagem de adicionar comando

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona as imagens a um servidor de serviços de implantação do Windows. Para obter exemplos de como você pode usar esse comando, consulte [exemplos](#BKMK_examples).
## <a name="syntax"></a>Sintaxe
para imagens de inicialização, use a seguinte sintaxe:
```
wdsutil /add-ImagmediaFile:<wim file path> [/Server:<Server name>mediatype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>] 
[/Filename:<New wim file name>]
```
para imagens de instalação, use a seguinte sintaxe:
```
wdsutil /add-ImagmediaFile:<wim file path>
     [/Server:<Server name>]
   mediatype:Install
     [/Skipverify]
    mediaGroup:<Image group name>]
     [/SingleImage:<Single image name>]
         [/Name:<Name>]
         [/Description:<Description>]
     [/Filename:<File name>]
     [/UnattendFile:<Unattend file path>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mediaFile: < caminho do arquivo. wim >|Especifica o nome de arquivo e caminho completo do arquivo de imagem do Windows (. wim) que contém as imagens a serem adicionados.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se um nome do servidor não for especificado, o servidor local será usado.|
mediatype:{Boot&#124;Install}|Especifica o tipo de imagens a serem adicionados.|
|[/Skipverify]|Especifica que a verificação de integridade não será executada no arquivo de imagem de origem antes de adicionar a imagem.|
|[/Name:<Name>]|Define o nome de exibição da imagem.|
|[/Description:<Description>]|Define a descrição da imagem.|
|[/Filename:<Filename>]|Especifica o novo nome de arquivo para o arquivo. wim. Isso permite que você altere o nome do arquivo do arquivo. wim ao adicionar a imagem. Se nenhum nome de arquivo for especificado, será usado o nome de arquivo de imagem de origem. Em todos os casos, os serviços de implantação do Windows verifica para determinar se o nome do arquivo é exclusivo no repositório de imagens de inicialização do computador de destino.|
|\mediaGroup:<Image group name>]|Especifica o nome do grupo de imagem na qual as imagens devem ser adicionados. Se houver mais de um grupo de imagens no servidor, o grupo de imagens deve ser especificado. Se isso não for especificado e um grupo de imagens ainda não existir, um novo grupo de imagens será criado. Caso contrário, o grupo de imagens existente será usado.|
|[/SingleImage:<Single image name>] [/Name:<Name>] [/Description:<Description>]|Copia a imagem única especificada fora de um arquivo. wim e, em seguida, define o nome de exibição da imagem e a descrição.|
|[/UnattendFile:<Unattend file path>]|Especifica o caminho completo para o arquivo de instalação autônoma a serem associados com as imagens que estão sendo adicionadas. Se **/SingleImage** não for especificado, o mesmo arquivo autônomo que será associado a todas as imagens no arquivo. wim.|
## <a name="BKMK_examples"></a>Exemplos
Para adicionar uma imagem de inicialização, digite:
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Boot.wimmediatype:Boot
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share\Boot.wim /Server:MyWDSServemediatype:Boot /Name:"My WinPE Image" 
/Description:"WinPE Image containing the WDS Client" /Filename:WDSBoot.wim
```
Para adicionar uma imagem de instalação, digite o seguinte:
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Install.wimmediatype:Install
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share \Install.wim /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/SingleImage:"Windows Pro" /Name:"My WDS Image"
/Description:"Windows Pro image with Microsoft Office" /Filename:"Win Pro.wim" /UnattendFile:"\\server\share\unattend.xml"
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando de cópia-Image](using-the-copy-image-command.md)
[usando o comando Export-Image](using-the-export-image-command.md)
[usando o Imagem do comando Get-](using-the-get-image-command.md)
[usando o comando de remove-Image](using-the-remove-image-command.md)
[usando a comando de substituir imagem](using-the-replace-image-command.md) 
 [ Subcomando: set-Image](subcommand-set-image.md)
