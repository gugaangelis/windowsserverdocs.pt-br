---
title: Add-DriverPackage
description: Artigo de referência para o comando Add-DriverPackage, que adiciona um pacote de driver ao servidor.
ms.topic: reference
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c85279cc160ca12b52f4cf7225d0ac41700973bc
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524512"
---
# <a name="add-driverpackage"></a>Add-DriverPackage

Adiciona um pacote de driver ao servidor.

## <a name="syntax"></a>Sintaxe

```
wdsutil /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /InfFile:`<InfFilepath>` | Especifica o caminho completo do arquivo. inf a ser adicionado. |
| [/Server:`<Servername>`] | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
| [/Architecture: `{x86 | ia64 | x64}` ] | Especifica o tipo de arquitetura para o pacote de driver. |
| [/DriverGroup: `<groupname>` ] | Especifica o nome do grupo de drivers ao qual os pacotes devem ser adicionados. |
| [/Name: `<friendlyname>` ] | Especifica o nome amigável para o pacote de driver. |

## <a name="examples"></a>Exemplos

Para adicionar um pacote de driver, digite:

```
wdsutil /verbose /Add-DriverPackage /InfFile:C:\Temp\Display.inf
```

```
wdsutil /Add-DriverPackage /Server:MyWDSServer /InfFile:C:\Temp\Display.inf /Architecture:x86 /DriverGroup:x86Drivers /Name:Display Driver
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando WDSUTIL Add-drivergrouppackage](wdsutil-add-drivergrouppackage.md)

- [comando WDSUTIL Add-AllDriverPackages](wdsutil-add-alldriverpackages.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
