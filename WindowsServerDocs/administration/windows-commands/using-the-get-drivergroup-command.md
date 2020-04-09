---
title: obter um controlador de driver
description: O tópico de comandos do Windows para Get-Driver, que exibe informações sobre os grupos de drivers em um servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cfe10c3-a63f-48e7-bef9-f6b474b4ddbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60a0649e98a0af0cbbd12db9a07f97dade66344c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831089"
---
# <a name="get-drivergroup"></a>obter um controlador de driver

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre os grupos de drivers em um servidor.

## <a name="syntax"></a>Sintaxe
```
wdsutil /Get-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/DriverGroup:<Group Name>|Especifica o nome do grupo de drivers.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN.  se um nome de servidor não for especificado, o servidor local será usado.|
|[/Show: {PackageMetaData &#124; Filters &#124; All}]|Exibe os metadados de todos os pacotes de driver no grupo especificado. **PackageMetaData** exibe informações sobre todos os filtros para o grupo de drivers. **Filtros** exibe os metadados de todos os pacotes de driver e filtros para o grupo.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para exibir informações sobre um arquivo de driver, digite:
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Show:PackageMetaData
```
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Server:MyWdsServer /Show:Filters
```
## <a name="additional-references"></a>Referências adicionais
- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando Get-AllDriverGroups](using-the-get-alldrivergroups-command.md)
