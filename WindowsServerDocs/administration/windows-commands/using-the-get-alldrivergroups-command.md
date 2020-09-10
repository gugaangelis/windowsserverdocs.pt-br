---
title: Get-AllDriverGroups
description: Artigo de referência para Get-AllDriverGroups, que exibe informações sobre todos os grupos de drivers em um servidor.
ms.topic: reference
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bb3c8c79e95bc5617e2eff0c168b54ba039a5c14
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638190"
---
# <a name="get-alldrivergroups"></a>Get-AllDriverGroups

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre todos os grupos de drivers em um servidor.

## <a name="syntax"></a>Sintaxe
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se um nome de servidor não for especificado, o servidor local será usado.|
|[/Show: {PackageMetaData &#124; filtros &#124; All}]|Exibe os metadados de todos os pacotes de driver no grupo especificado. **PackageMetaData** exibe informações sobre todos os filtros para o grupo de drivers. **Filtros** exibe os metadados de todos os pacotes de driver e filtros para o grupo.|
## <a name="examples"></a>Exemplos
Para exibir informações sobre um arquivo de driver, digite:
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando Get-driver](using-the-get-drivergroup-command.md)
