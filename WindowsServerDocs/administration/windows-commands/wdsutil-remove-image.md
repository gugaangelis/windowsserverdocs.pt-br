---
title: WDSUTIL remover-imagem
description: Artigo de referência para WDSUTIL Remove-Image, que exclui uma imagem de um servidor.
ms.topic: reference
ms.assetid: ce5e2384-2264-4b22-92af-74eec8c10ae0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f1d955f54f345b6d2832eabe2334dd77f32633e7
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729814"
---
# <a name="wdsutil-remove-image"></a>WDSUTIL remover-imagem

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui uma imagem de um servidor.

## <a name="syntax"></a>Syntax
para imagens de inicialização:
```
wdsutil [Options] /remove-Image:<Image name> [/Server:<Server name> type:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
```
para imagens de instalação:
```
wdsutil [Options] /remove-image:<Image name> [/Server:<Server name> type:Install ImageGroup:<Image group name>] [/Filename:<Filename>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
| /remove-image:<Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
MediaType: {boot &#124; install}|Especifica o tipo de imagem.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem. Como é possível ter o mesmo nome de imagem para diferentes imagens de inicialização em diferentes arquiteturas, especificar o valor da arquitetura garante que a imagem correta será removida.|
|\ImageGroup: <Image group name> ]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo de imagens será usado. Se houver mais de um grupo de imagens, você deverá usar essa opção para especificar o grupo de imagens.|
|[/Filename:<File name>]|se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
## <a name="examples"></a>Exemplos
Para remover uma imagem de inicialização, digite:
```
wdsutil /remove-Imagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86
```
```
wdsutil /verbose /remove-Image:WinPE Boot Image /Server:MyWDSServer type:Boot /Architecture:x64 /Filename:boot.wim
```
Para remover uma imagem de instalação, digite:
```
wdsutil /remove-Image:Windows Vista with Officemediatype:Install
```
```
wdsutil /verbose /remove-Image:Windows Vista with Office /Server:MyWDSServemediatype:Instal ImageGroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando de adição de imagem do WDSUTIL](wdsutil-add-image.md)
- [comando de cópia de imagem do WDSUTIL](wdsutil-copy-image.md)
- [comando WDSUTIL Export-Image](wdsutil-export-image.md)
- [comando de Get-Image do WDSUTIL](wdsutil-get-image.md)
- [comando de substituição do WDSUTIL-imagem](wdsutil-replace-image.md)
- [WDSUTIL Set-Command Image](wdsutil-set-image.md)
