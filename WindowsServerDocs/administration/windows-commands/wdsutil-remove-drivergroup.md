---
title: remover-controlador de driver
description: Artigo de referência para Remove-Group-Driver, que remove um grupo de drivers de um servidor.
ms.topic: reference
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 55cb50d43bf433d5421f4152053844f89d45f379
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729616"
---
# <a name="remove-drivergroup"></a>remover-controlador de driver

Remove um grupo de drivers de um servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/DriverGroup:\<Group Name>|Especifica o nome do grupo de drivers a ser removido.|
|[/Server:\<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se um nome de servidor não for especificado, o servidor local será usado.|

## <a name="examples"></a>Exemplos

Para remover um grupo de drivers, digite um dos seguintes:
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)