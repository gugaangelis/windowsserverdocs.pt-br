---
title: Get-MyImage
description: Artigo de referência para Get-Image Group, que recupera informações sobre um grupo de imagens e as imagens contidas nele.
ms.topic: reference
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c135072be3ff8ccf0993ed72ddf8f079f1d88c05
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638097"
---
# <a name="get-imagegroup"></a>Get-MyImage

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre um grupo de imagens e as imagens dentro dele.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
The Media:<Image group name>|Especifica o nome do grupo de imagens.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/detailed]|Retorna os metadados da imagem para cada imagem. Se esse parâmetro não for usado, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo.|
## <a name="examples"></a>Exemplos
Para exibir informações sobre um grupo de imagens, digite:
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Para exibir informações, incluindo metadados, digite:
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-add-imagegroup-command.md) 
 Add-imageus [Usando o comando](using-the-get-allimagegroups-command.md) 
 Get-AllImageGroups [Usando o comando](using-the-remove-imagegroup-command.md) 
 Remove-MyImage [Subcomando: Set-grupo de imagens](subcommand-set-imagegroup.md)
