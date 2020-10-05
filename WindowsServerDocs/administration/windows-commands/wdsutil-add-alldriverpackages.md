---
title: WDSUTIL Add-AllDriverPackages
description: Artigo de referência para WDSUTIL Add-AllDriverPackages, que adiciona todos os pacotes de driver armazenados em uma pasta a um servidor.
ms.topic: reference
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d3ec9b3d61c481eacd192ff74e149dbb56134b17
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729799"
---
# <a name="wdsutil-add-alldriverpackages"></a>WDSUTIL Add-AllDriverPackages

Adiciona todos os pacotes de driver armazenados em uma pasta a um servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

### <a name="parameters"></a>Parâmetros

|          Parâmetro           |                                                              Descrição                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  FolderPath\<Folder Path>  |                      Especifica o caminho completo para a pasta que contém os arquivos. inf para os pacotes de driver.                      |
|   [/Server:\<Server name>]   | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
|     [/Architecture: {x86      |                                                                 Win64                                                                  |
| [/DriverGroup: \<Group Name> ] |                             Especifica o nome do grupo de drivers ao qual os pacotes devem ser adicionados.                             |

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

- [wsdutil Add-wdsdriverpackage](/previous-versions/windows/powershell-scripting/dn283440(v=wps.630))
