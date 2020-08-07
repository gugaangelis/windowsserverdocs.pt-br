---
title: Get-AllImageGroups
description: Artigo de referência para Get-AllImageGroups, que recupera informações sobre todos os grupos de imagens em um servidor e todas as imagens nesses grupos de imagens.
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 523c761df06d11f6c1af5488370b540af9bc7326
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879826"
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
