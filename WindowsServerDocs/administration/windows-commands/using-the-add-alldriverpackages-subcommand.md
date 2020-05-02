---
title: Usando o subcomando Add-AllDriverPackages
description: Tópico de referência para Add-AllDriverPackages, que adiciona todos os pacotes de driver armazenados em uma pasta a um servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 31daa8fc3e3304dba5079672ea4619fd085dd74f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721164"
---
# <a name="add-alldriverpackages"></a>Add-AllDriverPackages

Adiciona todos os pacotes de driver armazenados em uma pasta a um servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

### <a name="parameters"></a>Parâmetros

|          Parâmetro           |                                                              Descrição                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  /FolderPath:\<caminho da pasta>  |                      Especifica o caminho completo para a pasta que contém os arquivos. inf para os pacotes de driver.                      |
|   [/Server:\<nome do servidor>]   | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
|     [/Architecture: {x86      |                                                                 Win64                                                                  |
| [/DriverGroup:\<nome do grupo>] |                             Especifica o nome do grupo de drivers ao qual os pacotes devem ser adicionados.                             |

## <a name="examples"></a>Exemplos

Para adicionar pacotes de driver, digite um dos seguintes:
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers\Printers /DriverGroup:Printer Drivers
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Add-WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)