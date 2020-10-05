---
title: imagem de cópia do WDSUTIL
description: Artigo de referência para WDSUTIL Copy-Image, que copia imagens que estão dentro do mesmo grupo de imagens.
ms.topic: reference
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a04848d0b673697809af08b91ca91856d6491773
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729693"
---
# <a name="wdsutil-copy-image"></a>imagem de cópia do WDSUTIL

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia imagens que estão dentro do mesmo grupo de imagens. Para copiar imagens entre grupos de imagens, use o comando [wdsutilExport-Image](wdsutil-export-image.md) e, em seguida, o comando [wdsutiladd-Image](wdsutil-add-image.md) .

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /copy-Image image:<Image name> [/Server:<Server name>]
    imagetype:Install
     imageGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Name:<Name>
         /Filename:<File name>
         [/Description:<Description>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
| imagem<Image name>|Especifica o nome da imagem a ser copiada.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
| ImageType: instalar|Especifica o tipo de imagem a ser copiada. Essa opção deve ser definida como **instalar**.|
|\imageGroup: <Image group name> ]|Especifica o grupo de imagens que contém a imagem a ser copiada. Se nenhum grupo de imagens for especificado e houver apenas um grupo no servidor, esse grupo de imagens será usado por padrão. Se houver mais de um grupo de imagens no servidor, você deverá especificar o grupo de imagens.|
|[/Filename:<Filename>]|Especifica o nome do arquivo da imagem a ser copiada. Se a imagem de origem não puder ser identificada exclusivamente pelo nome, você deverá especificar o nome do arquivo.|
|/DestinationImage|Especifica as configurações para a imagem de destino, conforme descrito na tabela a seguir.<p>-/Name: <Name> -define o nome de exibição da imagem a ser copiada.<br />-/Filename: <Filename> -define o nome do arquivo de imagem de destino que conterá a cópia da imagem.<br />- [/Description: <Description>] – Define a descrição da cópia da imagem.|
## <a name="examples"></a>Exemplos
Para criar uma cópia da imagem especificada e nomeá-la Aplicativoscompatíveis. wim, digite:
```
wdsutil /copy-Image image:Windows Vista with Office imagetype:Install /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim
```
Para criar uma cópia da imagem especificada, aplique as configurações especificadas e nomeie a cópia Aplicativoscompatíveis. wim, digite:
```
wdsutil /verbose /Progress /copy-Image image:Windows Vista with Office /Server:MyWDSServe imagetype:Install imageGroup:ImageGroup1
/Filename:install.wim /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim /Description:This is a copy of the original Windows image with Office installed
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando de adição de imagem do WDSUTIL](wdsutil-add-image.md)
- [comando WDSUTIL Export-Image](wdsutil-export-image.md)
- [comando de Get-Image do WDSUTIL](wdsutil-get-image.md)
- [comando de remoção de imagem do WDSUTIL](wdsutil-remove-image.md)
- [comando de substituição do WDSUTIL-imagem](wdsutil-replace-image.md)
- [WDSUTIL Set-Command Image](wdsutil-set-image.md)
