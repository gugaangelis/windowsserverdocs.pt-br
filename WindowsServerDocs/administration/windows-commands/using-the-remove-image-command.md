---
title: Usando o comando de remove-Image
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce5e2384-2264-4b22-92af-74eec8c10ae0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2aaf7d63d858045f9dd5df399c5f3f92b038bc2e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846697"
---
# <a name="using-the-remove-image-command"></a>Usando o comando de remove-Image

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui uma imagem de um servidor.
## <a name="syntax"></a>Sintaxe
para imagens de inicialização:
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
```
Para instalar imagens:
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<Filename>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mídia:<Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
tipo de mídia: {inicialização &#124; instalar}|Especifica o tipo de imagem.|
|/ Arquitetura: {x86 &#124; ia64 &#124; x64}|Especifica a arquitetura da imagem. Como é possível ter o mesmo nome de imagem para imagens de inicialização diferente em diferentes arquiteturas, especificando o valor de arquitetura garante que a imagem correta será removida.|
|\mediaGroup:<Image group name>]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagem for especificado e o grupo de apenas uma imagem existe no servidor, esse grupo será usado. Se houver mais de um grupo de imagens, você deve usar essa opção para especificar o grupo de imagens.|
|[/Filename:<File name>]|Se a imagem não pode ser identificada exclusivamente pelo nome, você deve usar essa opção para especificar o nome do arquivo.|
## <a name="BKMK_examples"></a>Exemplos
Para remover uma imagem de inicialização, digite:
```
wdsutil /remove-Imagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86
```
```
wdsutil /verbose /remove-Imagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim
```
Para remover uma imagem de instalação, digite:
```
wdsutil /remove-Imagmedia:"Windows Vista with Officemediatype:Install
```
```
wdsutil /verbose /remove-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-Image](using-the-add-image-command.md)
[usando a comando de copiar imagem](using-the-copy-image-command.md)
[usando o Comando de Export-Image](using-the-export-image-command.md)
[usando o comando get-Image](using-the-get-image-command.md)
[usando a comando de substituir imagem](using-the-replace-image-command.md) 
 [ Subcomando: set-Image](subcommand-set-image.md)
