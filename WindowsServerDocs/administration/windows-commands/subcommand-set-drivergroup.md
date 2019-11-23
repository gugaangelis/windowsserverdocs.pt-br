---
title: Conjunto de subcomandos – grupo de drivers
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 751985cffea32b5129909576f0631cce83adc9a2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370839"
---
# <a name="subcommand-set-drivergroup"></a>Subcomando: Set-grupo de driver

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define as propriedades de um grupo de drivers existente em um servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/DriverGroup:<Group Name>|Especifica o nome do grupo de drivers.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se um nome de servidor não for especificado, o servidor local será usado.|
|[/Name:<New Group Name>]|Especifica o novo nome para o grupo de drivers.|
|[/Enabled: {Sim &#124; não}|Habilita ou desabilita o grupo de drivers.|
|[/Applicability: {MATCHED &#124; All}]|Especifica quais pacotes instalar se os critérios de filtro forem atendidos. **MATCHED** significa instalar apenas os pacotes de driver que correspondem a um hardware de cliente. **Tudo** significa instalar todos os pacotes para clientes, independentemente de seu hardware.|
## <a name="BKMK_examples"></a>Disso
Para definir as propriedades de um grupo de drivers, digite um dos seguintes:
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
#### <a name="additional-references"></a>referências adicionais
Subcomando
[chave de sintaxe de linha de comando](command-line-syntax-key.md) [: Set-DriverGroupFilter](subcommand-set-drivergroupfilter.md)
