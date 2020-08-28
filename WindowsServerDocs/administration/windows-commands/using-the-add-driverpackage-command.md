---
title: Add-DriverPackage
description: Artigo de referência para Add-DriverPackage, que adiciona um pacote de driver ao servidor.
ms.topic: reference
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a1876727d4d79cf4ce3c86c654a9be0b7c6795d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029844"
---
# <a name="add-driverpackage"></a>Add-DriverPackage

Adiciona um pacote de driver ao servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

### <a name="parameters"></a>Parâmetros

|          Parâmetro           |                                                              Descrição                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|   InfFile:\<Inf File path>   |                                           Especifica o caminho completo do arquivo. inf a ser adicionado.                                            |
|    Servidor\<Server name>    | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
|      /Architecture: {x86      |                                                                 Win64                                                                  |
| [/DriverGroup: \<Group Name> ] |                             Especifica o nome do grupo de drivers ao qual o pacote deve ser adicionado.                              |
|   [/Name: \<Friendly Name> ]   |                                           Declara o nome amigável para o pacote de driver.                                            |

## <a name="examples"></a>Exemplos

Para adicionar um pacote de driver, digite um dos seguintes:
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:C:\Temp\Display.inf
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:C:\Temp\Display.inf /Architecture:x86 /DriverGroup:x86Drivers /Name:Display Driver
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

