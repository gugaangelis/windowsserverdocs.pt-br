---
title: Subcomando DriverGroup conjunto
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e645e16a3d78dd91bad98fedbb04896025b0eaf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852697"
---
# <a name="subcommand-set-drivergroup"></a>Subcommand: set-DriverGroup

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define as propriedades de um grupo de driver existente em um servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/DriverGroup:<Group Name>|Especifica o nome do grupo de drivers.|
|[/Server:<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou FQDN. Se não for especificado um nome de servidor, o servidor local será usado.|
|[/Name:<New Group Name>]|Especifica o novo nome para o grupo de drivers.|
|[/ Habilitado: {Sim &#124; não}|Habilita ou desabilita o grupo de drivers.|
|[/Applicability:{Matched &#124; All}]|Especifica quais pacotes a serem instalados se os critérios de filtro for atendido. **Correspondido** significa instalar apenas os pacotes de driver que correspondem a um hardware cliente s. **Todos os** significa instala todos os pacotes aos clientes, independentemente de seu hardware.|
## <a name="BKMK_examples"></a>Exemplos
Para definir as propriedades de um grupo de drivers, digite um dos seguintes:
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[subcomando: set-DriverGroupFilter](subcommand-set-drivergroupfilter.md)
