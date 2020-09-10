---
title: Get-AllImageGroups
description: Artigo de referência para Get-AllImageGroups, que recupera informações sobre todos os grupos de imagens em um servidor e todas as imagens nesses grupos de imagens.
ms.topic: reference
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b69375cff867d317c3ac6c83ec1c75358455ac03
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626424"
---
# <a name="get-allimagegroups"></a>Get-AllImageGroups

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre todos os grupos de imagens em um servidor e todas as imagens nesses grupos de imagens.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/detailed]|Retorna os metadados da imagem de cada imagem. Se esse parâmetro não for usado, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo para cada imagem.|
## <a name="examples"></a>Exemplos
Para exibir informações sobre os grupos de imagens, digite um dos seguintes:
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-add-imagegroup-command.md) 
 Add-imageus [Usando o comando](using-the-get-imagegroup-command.md) 
 Get-imageus [Usando o comando](using-the-remove-imagegroup-command.md) 
 Remove-MyImage [Subcomando: Set-grupo de imagens](subcommand-set-imagegroup.md)
