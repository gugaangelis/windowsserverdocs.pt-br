---
title: Add-DriverGroupFilter
description: Artigo de referência para o comando Add-DriverGroupFilter, que adiciona um filtro a um grupo de drivers em um servidor.
ms.topic: reference
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bd9e26e7bdf6b1a03d01b2993c969bbbcf5b8754
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524601"
---
# <a name="add-drivergroupfilter"></a>Add-DriverGroupFilter

Adiciona um filtro a um grupo de drivers em um servidor.

## <a name="syntax"></a>Sintaxe

```
wdsutil /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /DriverGroup:`<Groupname>` | Especifica o nome do novo grupo de drivers. |
| Servidor`<Servername>` | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado. |
| FilterType`<Filtertype>` | Especifica o tipo do filtro a ser adicionado ao grupo. Você pode especificar vários tipos de filtro em um único comando. Cada tipo de filtro deve ser seguido por **/Policy** e pelo menos um **/Value**. Os valores válidos incluem:<ul><li>BiosVendor</li><li>Biosversion</li><li>Chassistype</li><li>Fabricante</li><li>Uuid</li><li>OSVersion</li><li>Osedition</li><li>OsLanguage</li></ul> Para obter informações sobre como obter valores para todos os outros tipos de filtro, consulte [filtros de grupo de driver](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759191(v=ws.11)). |
| [/Policy: `{Include|Exclude}` ] | Especifica a política a ser definida no filtro. Se **/Policy** estiver definido como **include**, os computadores cliente que correspondem ao filtro terão permissão para instalar os drivers nesse grupo. Se **/Policy** for definido como **Exclude**, os computadores cliente que corresponderem ao filtro não terão permissão para instalar os drivers nesse grupo. |
| [/Value: `<Value>` ] | Especifica o valor do cliente que corresponde a **/FilterType**. Você pode especificar vários valores para um único tipo. Para obter informações sobre valores de tipo de filtro aceitáveis, consulte [filtros de grupo de driver](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759191(v=ws.11)). |

## <a name="examples"></a>Exemplos

Para adicionar um filtro a um grupo de drivers, digite:

```
wdsutil /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```

```
wdsutil /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando WDSUTIL Add-drivergrouppackage](wdsutil-add-drivergrouppackage.md)

- [comando WDSUTIL Add-drivergrouppackages](wdsutil-add-drivergrouppackages.md)

- [comando WDSUTIL Add-driver](wdsutil-add-drivergroup.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
