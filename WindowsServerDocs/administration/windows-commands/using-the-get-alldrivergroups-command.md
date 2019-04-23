---
title: Usando o comando get-AllDriverGroups
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 236a2f798fb07ee6eafb9baf9314dbf46a984cdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873997"
---
# <a name="using-the-get-alldrivergroups-command"></a>Usando o comando get-AllDriverGroups

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre todos os grupos de driver em um servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou FQDN. Se não for especificado um nome de servidor, o servidor local será usado.|
|[/Show: {PackageMetaData &#124; Filters &#124; All}]|Exibe os metadados para todos os pacotes de driver no grupo especificado. **PackageMetaData** exibe informações sobre todos os filtros para o grupo de drivers. **Filtros** exibe os metadados para todos os pacotes de driver e os filtros para o grupo.|
## <a name="BKMK_examples"></a>Exemplos
Para exibir informações sobre um arquivo de driver, digite:
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando get-DriverGroup](using-the-get-drivergroup-command.md)
