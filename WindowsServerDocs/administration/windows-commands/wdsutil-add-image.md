---
title: Add-Image do WDSUTIL
description: Artigo de referência do comando WDSUTIL Add-Image, que adiciona imagens a um servidor dos serviços de implantação do Windows.
ms.topic: reference
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e0d80c0a3aa9cfc6a5b9e05f11542dfcc8621946
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524511"
---
# <a name="wdsutil-add-image"></a>Add-Image do WDSUTIL

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona imagens a um servidor dos serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe

Para imagens de inicialização, use a seguinte sintaxe:

```
wdsutil /add-Image imageFile:<wim file path> [/Server:<Server name> imagetype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>] [/Filename:<New wim file name>]
```

Para imagens de instalação, use a seguinte sintaxe:

```
wdsutil /add-Image imageFile:<wim filepath> [/Server:<Servername>] imagetype:Install [/Skipverify] imageGroup:<Image group name>] [/SingleImage:<Single image name>] [/Name:<Name>] [/Description:<Description>] [/Filename:<File name>] [/UnattendFile:<Unattend file path>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| ImageFile:`<.wim filepath>` | Especifica o caminho completo e o nome de arquivo do arquivo de imagem do Windows (. wim) que contém as imagens a serem adicionadas. |
| [/Server:`<Servername>`] | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se um nome de servidor não for especificado, o servidor local será usado. |
| ImageType`{Boot|Install}` | Especifica o tipo de imagens a serem adicionadas. |
| [/Skipverify] | Especifica que a verificação de integridade não será executada no arquivo de imagem de origem antes que a imagem seja adicionada. |
| [/Name: `<Name>` ] | Define o nome de exibição da imagem. |
| [/Description: `<Description>` ] | Define a descrição da imagem. |
| [/Filename:`<Filename>`] | Especifica o novo nome de arquivo para o arquivo. wim. Isso permite que você altere o nome do arquivo. wim ao adicionar a imagem. Se você não especificar um nome de arquivo, o nome de arquivo da imagem de origem será usado. Em todos os casos, os serviços de implantação do Windows verificam se o nome do arquivo é exclusivo no repositório de imagens de inicialização do computador de destino. |
| \imageGroup: `<Imagegroupname>` ] | Especifica o nome do grupo de imagens no qual as imagens devem ser adicionadas. Se houver mais de um grupo de imagens no servidor, o grupo de imagens deverá ser especificado. Se você não especificar o grupo de imagens e um grupo de imagens ainda não existir, um novo grupo de imagens será criado. Caso contrário, o grupo de imagens existente será usado. |
| [/SingleImage: `<Singleimagename>` ] [/Name: `<Name>` ] [/Description: `<Description>` ] | Copia a imagem única especificada de um arquivo. wim e define o nome de exibição e a descrição da imagem. |
| [/UnattendFile:`<Unattendfilepath>`] | Especifica o caminho completo para o arquivo de instalação autônoma a ser associado às imagens que estão sendo adicionadas. Se **/SingleImage** não for especificado, o mesmo arquivo autônomo será associado a todas as imagens no arquivo. wim. |

## <a name="examples"></a>Exemplos

Para adicionar uma imagem de inicialização, digite:

```
wdsutil /add-Image imageFile:C:\MyFolder\Boot.wim imagetype:Boot
wdsutil /verbose /Progress /add-Image imageFile:\\MyServer\Share\Boot.wim /Server:MyWDSServer imagetype:Boot /Name:My WinPE Image /Description:WinPE Image containing the WDS Client /Filename:WDSBoot.wim
```

Para adicionar uma imagem de instalação, digite uma das seguintes opções:

```
wdsutil /add-Image imageFile:C:\MyFolder\Install.wim imagetype:Install
wdsutil /verbose /Progress /add-Image imageFile:\\MyServer\Share \Install.wim /Server:MyWDSServer imagetype:Instal imageGroup:ImageGroup1
/SingleImage:Windows Pro /Name:My WDS Image /Description:Windows Pro image with Microsoft Office /Filename:Win Pro.wim /UnattendFile:\\server\share\unattend.xml
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de cópia de imagem do WDSUTIL](wdsutil-copy-image.md)

- [comando WDSUTIL Export-Image](wdsutil-export-image.md)

- [comando de Get-Image do WDSUTIL](wdsutil-get-image.md)

- [comando de remoção de imagem do WDSUTIL](wdsutil-remove-image.md)

- [comando de substituição do WDSUTIL-imagem](wdsutil-replace-image.md)

- [WDSUTIL Set-Command Image](wdsutil-set-image.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
