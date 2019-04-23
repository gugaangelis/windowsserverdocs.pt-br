---
title: Usando o comando get-DriverGroup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cfe10c3-a63f-48e7-bef9-f6b474b4ddbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f82969e03b3474cf39afd2ae5c3ef2f9d4f8b5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847147"
---
# <a name="using-the-get-drivergroup-command"></a>Usando o comando get-DriverGroup

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre grupos de drivers em um servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil /Get-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/DriverGroup:<Group Name>|Especifica o nome do grupo de drivers.|
|[/Server:<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou FQDN.  Se não for especificado um nome de servidor, o servidor local será usado.|
|[/Show: {PackageMetaData &#124; Filters &#124; All}]|Exibe os metadados para todos os pacotes de driver no grupo especificado. **PackageMetaData** exibe informações sobre todos os filtros para o grupo de drivers. **Filtros** exibe os metadados para todos os pacotes de driver e os filtros para o grupo.|
## <a name="BKMK_examples"></a>Exemplos
Para exibir informações sobre um arquivo de driver, digite:
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Show:PackageMetaData
```
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Server:MyWdsServer /Show:Filters
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando get-AllDriverGroups](using-the-get-alldrivergroups-command.md)
