---
title: Usando o comando Get-AllDriverGroups
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bed6c784b2fafa30f2beb0394b64fe570ddd8ff7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363383"
---
# <a name="using-the-get-alldrivergroups-command"></a>Usando o comando Get-AllDriverGroups

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre todos os grupos de drivers em um servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se um nome de servidor não for especificado, o servidor local será usado.|
|[/Show: {PackageMetaData &#124; Filters &#124; All}]|Exibe os metadados de todos os pacotes de driver no grupo especificado. **PackageMetaData** exibe informações sobre todos os filtros para o grupo de drivers. **Filtros** exibe os metadados de todos os pacotes de driver e filtros para o grupo.|
## <a name="BKMK_examples"></a>Disso
Para exibir informações sobre um arquivo de driver, digite:
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando Get-driver](using-the-get-drivergroup-command.md)
