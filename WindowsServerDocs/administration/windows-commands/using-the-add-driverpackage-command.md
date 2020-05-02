---
title: Add-DriverPackage
description: Tópico de referência para Add-DriverPackage, que adiciona um pacote de driver ao servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12d7f7078cf3dde10f834a4d4c7784ecc1d9bdf2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721089"
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
|   InfFile:\<caminho do arquivo inf>   |                                           Especifica o caminho completo do arquivo. inf a ser adicionado.                                            |
|    /Server:\<nome do servidor>    | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
|      /Architecture: {x86      |                                                                 Win64                                                                  |
| [/DriverGroup:\<nome do grupo>] |                             Especifica o nome do grupo de drivers ao qual o pacote deve ser adicionado.                              |
|   [/Name:\<nome amigável>]   |                                           Declara o nome amigável para o pacote de driver.                                            |

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

