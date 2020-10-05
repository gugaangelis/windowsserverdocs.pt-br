---
title: Add-Image do WDSUTIL
description: Artigo de referência para o WDSUTIL Add-Image, que adiciona imagens a um servidor de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ef8068f45a8a2d49715420c06859d326bf93200a
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729712"
---
# <a name="wdsutil-add-image"></a>Add-Image do WDSUTIL

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona imagens a um servidor dos serviços de implantação do Windows.

## <a name="syntax"></a>Syntax
para imagens de inicialização, use a seguinte sintaxe:
```
wdsutil /add-Image imageFile:<wim file path> [/Server:<Server name> imagetype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>]
[/Filename:<New wim file name>]
```
para imagens de instalação, use a seguinte sintaxe:
```
wdsutil /add-Image imageFile:<wim file path>
     [/Server:<Server name>]
    imagetype:Install
     [/Skipverify]
     imageGroup:<Image group name>]
     [/SingleImage:<Single image name>]
         [/Name:<Name>]
         [/Description:<Description>]
     [/Filename:<File name>]
     [/UnattendFile:<Unattend file path>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|ImageFile: caminho do arquivo <. wim>|Especifica o caminho completo e o nome de arquivo do arquivo de imagem do Windows (. wim) que contém as imagens a serem adicionadas.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se um nome do servidor não for especificado, o servidor local será usado.|
| ImageType: {inicialização&#124;instalação}|Especifica o tipo de imagens a serem adicionadas.|
|[/Skipverify]|Especifica que a verificação de integridade não será executada no arquivo de imagem de origem antes que a imagem seja adicionada.|
|[/Name: <Name> ]|Define o nome de exibição da imagem.|
|[/Description:<Description>]|Define a descrição da imagem.|
|[/Filename:<Filename>]|Especifica o novo nome de arquivo para o arquivo. wim. Isso permite que você altere o nome do arquivo. wim ao adicionar a imagem. Se nenhum nome de arquivo for especificado, o nome do arquivo de imagem de origem será usado. Em todos os casos, os serviços de implantação do Windows verificam se o nome do arquivo é exclusivo no repositório de imagens de inicialização do computador de destino.|
|\imageGroup: <Image group name> ]|Especifica o nome do grupo de imagens no qual as imagens devem ser adicionadas. Se houver mais de um grupo de imagens no servidor, o grupo de imagens deverá ser especificado. Se isso não for especificado e um grupo de imagens ainda não existir, um novo grupo de imagens será criado. Caso contrário, o grupo de imagens existente será usado.|
|[/SingleImage: <Single image name> ] [/Name: <Name> ] [/Description:<Description>]|Copia a imagem única especificada de um arquivo. wim e define o nome de exibição e a descrição da imagem.|
|[/UnattendFile:<Unattend file path>]|Especifica o caminho completo para o arquivo de instalação autônoma a ser associado às imagens que estão sendo adicionadas. Se **/SingleImage** não for especificado, o mesmo arquivo autônomo será associado a todas as imagens no arquivo. wim.|
## <a name="examples"></a>Exemplos
Para adicionar uma imagem de inicialização, digite:
```
wdsutil /add-Image imageFile:C:\MyFolder\Boot.wim imagetype:Boot
wdsutil /verbose /Progress /add-Image imageFile:\\MyServer\Share\Boot.wim /Server:MyWDSServer imagetype:Boot /Name:My WinPE Image
/Description:WinPE Image containing the WDS Client /Filename:WDSBoot.wim
```
Para adicionar uma imagem de instalação, digite uma das seguintes opções:
```
wdsutil /add-Image imageFile:C:\MyFolder\Install.wim imagetype:Install
wdsutil /verbose /Progress /add-Image imageFile:\\MyServer\Share \Install.wim /Server:MyWDSServer imagetype:Instal imageGroup:ImageGroup1
/SingleImage:Windows Pro /Name:My WDS Image
/Description:Windows Pro image with Microsoft Office /Filename:Win Pro.wim /UnattendFile:\\server\share\unattend.xml
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando de cópia de imagem do WDSUTIL](wdsutil-copy-image.md)
- [comando WDSUTIL Export-Image](wdsutil-export-image.md)
- [comando de Get-Image do WDSUTIL](wdsutil-get-image.md)
- [comando de remoção de imagem do WDSUTIL](wdsutil-remove-image.md)
- [comando de substituição do WDSUTIL-imagem](wdsutil-replace-image.md)
- [WDSUTIL Set-Command Image](wdsutil-set-image.md)
