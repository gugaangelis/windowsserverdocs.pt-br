---
title: Conjunto de subcomandos-DriverGroupFilter
description: Tópico de referência para subcomando set-DriverGroupFilter, que adiciona ou remove um filtro de grupo de drivers existente de um grupo de drivers.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8a8d3567785b54a722b1787c519de7eb0b7a229
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721730"
---
# <a name="subcommand-set-drivergroupfilter"></a>Subcomando: Set-DriverGroupFilter

Adiciona ou remove um filtro de grupo de drivers existente de um grupo de drivers.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

### <a name="parameters"></a>Parâmetros

|         Parâmetro          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:\<nome do grupo> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                 Especifica o nome do grupo de drivers.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|  [/Server:\<nome do servidor>]  |                                                                                                                                                                                                                                                                                                                                                                                                                Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se um nome de servidor não for especificado, o servidor local será usado.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| /FilterType:\<filtertype>  |                                                                                                                                                                                                                                                                       Especifica o tipo de filtro de grupo de driver a ser adicionado ou removido. Você pode especificar vários filtros em um único comando. Para cada **/FilterType**, você pode adicionar ou remover vários valores usando **/RemoveValue** e **/AddValue**. \<O FilterType> pode ser um dos seguintes:</br>**BiosVendor**</br>**Biosversion**</br>**ChassisType**</br>**Fabricante**</br>**Personalizado**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**                                                                                                                                                                                                                                                                        |
|     [/Policy: {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                Excluir}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|    [/AddValue:\<valor>]    | Especifica o novo valor do cliente a ser adicionado ao filtro. Você pode especificar vários valores para um único tipo de filtro. Consulte a lista a seguir para obter os valores de atributo válidos para **ChassisType**. Para obter informações sobre como obter os valores para todos os outros tipos de filtro, consulte<https://go.microsoft.com/fwlink/?LinkID=155158>filtros de grupo de [Driver](https://go.microsoft.com/fwlink/?LinkID=155158) ().</br>**Outros**</br>**UnknownChassis**</br>**Área de Trabalho**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Torre**</br>**Portáteis**</br>**Laptop**</br>**Notebook**</br>**Aparelhos**</br>**DockingStation**</br>**AllInOne**</br>**Subnotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Subchassi**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |
|  [/RemoveValue:\<valor>]   |                                                                                                                                                                                                                                                                                                                                                                                                                                     Especifica o valor do cliente existente a ser removido do filtro, conforme especificado com **/AddValue**.                                                                                                                                                                                                                                                                                                                                                                                                                                      |

## <a name="examples"></a>Exemplos

Para remover um filtro, digite um dos seguintes:
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)