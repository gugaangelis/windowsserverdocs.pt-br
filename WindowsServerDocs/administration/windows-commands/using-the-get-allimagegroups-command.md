---
title: Usando o comando get-AllImageGroups
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 917f61327a3d39ee97c5fd59072884f7844c487e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822347"
---
# <a name="using-the-get-allimagegroups-command"></a>Usando o comando get-AllImageGroups

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre todos os grupos de imagens em um servidor e todas as imagens desses grupos de imagem.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/ detalhadas]|Retorna os metadados de imagem de cada imagem. Se esse parâmetro não for usado, o comportamento padrão é retornar somente o nome da imagem, descrição e nome de arquivo para cada imagem.|
## <a name="BKMK_examples"></a>Exemplos
Para exibir informações sobre os grupos de imagem, digite um dos seguintes:
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-ImageGroup](using-the-add-imagegroup-command.md)
[usando o comando get-ImageGroup](using-the-get-imagegroup-command.md)
[usando o Comando de remove-ImageGroup](using-the-remove-imagegroup-command.md)
[subcomando: set-ImageGroup](subcommand-set-imagegroup.md)
