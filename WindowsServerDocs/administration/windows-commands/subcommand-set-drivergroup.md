---
title: Conjunto de subcomandos – grupo de drivers
description: Tópico de comandos do Windows para subcomando set-Group-Group, que define as propriedades de um grupo de drivers existente em um servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c49381dc65f3b2ffc9a04e4fb2699818515a931f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834029"
---
# <a name="subcommand-set-drivergroup"></a>Subcomando: Set-grupo de driver

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define as propriedades de um grupo de drivers existente em um servidor.

## <a name="syntax"></a>Sintaxe
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/DriverGroup:<Group Name>|Especifica o nome do grupo de drivers.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. se um nome de servidor não for especificado, o servidor local será usado.|
|[/Name:<New Group Name>]|Especifica o novo nome para o grupo de drivers.|
|[/Enabled: {Sim &#124; não}|Habilita ou desabilita o grupo de drivers.|
|[/Applicability: {MATCHED &#124; All}]|Especifica quais pacotes instalar se os critérios de filtro forem atendidos. **MATCHED** significa instalar apenas os pacotes de driver que correspondem a um hardware de cliente. **Tudo** significa instalar todos os pacotes para clientes, independentemente de seu hardware.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para definir as propriedades de um grupo de drivers, digite um dos seguintes:
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
## <a name="additional-references"></a>Referências adicionais
- Subcomando
[chave de sintaxe de linha de comando](command-line-syntax-key.md) [: Set-DriverGroupFilter](subcommand-set-drivergroupfilter.md)
