---
title: WDSUTIL Add-AllDriverPackages
description: Artigo de referência para o comando WDSUTIL Add-AllDriverPackages, que adiciona todos os pacotes de driver armazenados em uma pasta a um servidor.
ms.topic: reference
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 07d958fb382f040db34f486a28fed4b924ce45d8
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524651"
---
# <a name="wdsutil-add-alldriverpackages"></a>WDSUTIL Add-AllDriverPackages

Adiciona todos os pacotes de driver armazenados em uma pasta a um servidor.

## <a name="syntax"></a>Sintaxe

```
wdsutil /Add-AllDriverPackages /FolderPath:<folderpath> [/Server:<servername>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<groupname>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| FolderPath`<folderpath>` | Especifica o caminho completo para a pasta que contém os arquivos. inf para os pacotes de driver. |
| [/Server:`<servername>`] | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
| [/Architecture: `{x86|ia64|x64}` ] | Especifica o tipo de arquitetura para o pacote de driver. |
| [/DriverGroup: `<groupname>` ] | Especifica o nome do grupo de drivers ao qual os pacotes devem ser adicionados. |

## <a name="examples"></a>Exemplos

Para adicionar pacotes de driver, digite:

```
wdsutil /verbose /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers /Architecture:x86
```

```
wdsutil /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers\Printers /DriverGroup:Printer Drivers
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)

- [Add-WdsDriverPackage](/powershell/module/wds/add-wdsdriverpackage)
