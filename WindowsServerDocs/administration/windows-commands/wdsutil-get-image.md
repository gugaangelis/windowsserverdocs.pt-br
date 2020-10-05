---
title: WDSUTIL Get-Image
description: Artigo de referência para WDSUTIL Get-Image, que recupera informações sobre uma imagem.
ms.topic: reference
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0f6cfcf3783ef49f2dd57941ae9a2ae4feb3742e
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729644"
---
# <a name="wdsutil-get-image"></a>WDSUTIL Get-Image

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre uma imagem.

## <a name="syntax"></a>Syntax
para imagens de inicialização:
```
wdsutil [Options] /Get-Image image:<Image name> [/Server:<Server name> imagetype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
para imagens de instalação:
```
wdsutil [Options] /Get-image image:<Image name> [/Server:<Server name> imagetype:Install imagegroup:<Image group name>] [/Filename:<File name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
| imagem<Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
| ImageType: {inicialização &#124; instalação}|Especifica o tipo de imagem.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, especificar o valor da arquitetura garante que a imagem correta seja retornada.|
|[/Filename:<File name>]|se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
|\imagegroup: <Image group name> ]|Especifica o grupo de imagens que contém a imagem. Se nenhum grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo será usado. Se houver mais de um grupo de imagens no servidor, você deverá usar esse parâmetro para especificar o grupo de imagens.|
## <a name="examples"></a>Exemplos
Para recuperar informações sobre uma imagem de inicialização, digite um dos seguintes:
```
wdsutil /Get-Image image:WinPE boot imagetype:Boot /Architecture:x86
wdsutil /verbose /Get-Image image:WinPE boot image /Server:MyWDSServer imagetype:Boot /Architecture:x86 /Filename:boot.wim
```
Para recuperar informações sobre uma imagem de instalação, digite um dos seguintes:
```
wdsutil /Get-Image:Windows Vista with Office imagetype:Install
wdsutil /verbose /Get-Image:Windows Vista with Office /Server:MyWDSServer imagetype:Install imagegroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando de adição de imagem do WDSUTIL](wdsutil-add-image.md)
- [comando de cópia de imagem do WDSUTIL](wdsutil-copy-image.md)
- [comando WDSUTIL Export-Image](wdsutil-export-image.md)
- [comando de remoção de imagem do WDSUTIL](wdsutil-remove-image.md)
- [comando de substituição do WDSUTIL-imagem](wdsutil-replace-image.md)
- [WDSUTIL Set-Command Image](wdsutil-set-image.md)
