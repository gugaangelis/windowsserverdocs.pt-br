---
title: Adicionar um MyImage
description: Artigo de referência para Add-Image Group, que adiciona um grupo de imagens a um servidor de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 45edd86f62493cadbfbfd6e3f1e9bc86ea5bd9fc
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622216"
---
# <a name="add-imagegroup"></a>Adicionar um MyImage

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona um grupo de imagens a um servidor dos serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
The Media:<Image group name>|Especifica o nome do grupo de imagens a ser adicionado.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se um nome do servidor não for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para adicionar um grupo de imagens, digite um dos seguintes:
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-get-allimagegroups-command.md) 
 Get-AllImageGroups [Usando o comando](using-the-get-imagegroup-command.md) 
 Get-imageus [Usando o comando](using-the-remove-imagegroup-command.md) 
 Remove-MyImage [Subcomando: Set-grupo de imagens](subcommand-set-imagegroup.md)
