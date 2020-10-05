---
title: Conjunto de subcomandos-DriverGroupFilter
description: Artigo de referência para o subcomando set-DriverGroupFilter, que adiciona ou remove um filtro de grupo de drivers existente de um grupo de drivers.
ms.topic: reference
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a27795fb6a4f6851a8b114f3ce1f91560df9ce76
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729827"
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
| /DriverGroup:\<Group Name> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                 Especifica o nome do grupo de drivers.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|  [/Server:\<Server name>]  |                                                                                                                                                                                                                                                                                                                                                                                                                Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se um nome de servidor não for especificado, o servidor local será usado.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| FilterType\<FilterType>  |                                                                                                                                                                                                                                                                       Especifica o tipo de filtro de grupo de driver a ser adicionado ou removido. Você pode especificar vários filtros em um único comando. Para cada **/FilterType**, você pode adicionar ou remover vários valores usando **/RemoveValue** e **/AddValue**. \<FilterType> pode ser um dos seguintes:</br>**BiosVendor**</br>**Biosversion**</br>**ChassisType**</br>**Manufacturer**</br>**Personalizado**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**                                                                                                                                                                                                                                                                        |
|     [/Policy: {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                Excluir}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|    [/AddValue: \<Value> ]    | Especifica o novo valor do cliente a ser adicionado ao filtro. Você pode especificar vários valores para um único tipo de filtro. Consulte a lista a seguir para obter os valores de atributo válidos para **ChassisType**. Para obter informações sobre como obter os valores para todos os outros tipos de filtro, consulte [filtros de grupo de driver](https://go.microsoft.com/fwlink/?LinkID=155158) ( <https://go.microsoft.com/fwlink/?LinkID=155158> ).</br>**Outras**</br>**UnknownChassis**</br>**Área de Trabalho**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Torre**</br>**Portáteis**</br>**Laptop**</br>**Notebook**</br>**Aparelhos**</br>**DockingStation**</br>**AllInOne**</br>**Subnotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Subchassi**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |
|  [/RemoveValue: \<Value> ]   |                                                                                                                                                                                                                                                                                                                                                                                                                                     Especifica o valor do cliente existente a ser removido do filtro, conforme especificado com **/AddValue**.                                                                                                                                                                                                                                                                                                                                                                                                                                      |

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