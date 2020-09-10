---
title: copiar-controlador de driver
description: Artigo de referência para Copy-driver Group, que duplica um grupo de drivers existente no servidor, incluindo os filtros, pacotes de driver e status habilitado/desabilitado.
ms.topic: reference
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fee0b3b6cf27cd0bf04f93f61301f65db17ee4be
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622160"
---
# <a name="copy-drivergroup"></a>copiar-controlador de driver

Duplica um grupo de drivers existente no servidor, incluindo os filtros, os pacotes de driver e o status habilitado/desabilitado.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server:\<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/DriverGroup:\<Source Group Name>|Especifica o nome do grupo de drivers de origem.|
|GroupName\<New Group Name>|Especifica o nome do novo grupo de drivers.|

## <a name="examples"></a>Exemplos

Para copiar um grupo de drivers, digite um dos seguintes:
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)