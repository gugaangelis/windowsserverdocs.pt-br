---
title: Add-DriverGroupFilter
description: Artigo de referência para Add-DriverGroupFilter, que adiciona um filtro a um grupo de drivers em um servidor.
ms.topic: reference
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bf11cbe86242a8051b173aa23f53748c20aa4ca6
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729715"
---
# <a name="add-drivergroupfilter"></a>Add-DriverGroupFilter

Adiciona um filtro a um grupo de drivers em um servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

### <a name="parameters"></a>Parâmetros

|         Parâmetro          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:\<Group Name> |                                                                                                                                                                                                                                                                                                                                                                                                                                                              Especifica o nome do grupo de drivers.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|  [/Server:\<Server name>]  |                                                                                                                                                                                                                                                                                                                                                                                                               Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                                                                                                                                                                                                                                                                                                                               |
| FilterType\<FilterType>  |                                                                                                                                                                                                   Especifica o tipo de filtro a ser adicionado ao grupo. Você pode especificar vários tipos de filtro em um único comando. Cada tipo de filtro deve ser seguido por **/Policy** e incluir pelo menos um **/Value**. \<FilterType> pode ser **BiosVendor**, **biosversion**, **ChassisType**, **manufacturer**, **UUID**, **OsVersion**, **OsEdition**ou **OsLanguage**. Para obter informações sobre como obter os valores para todos os outros tipos de filtro, consulte [filtros de grupo de driver](https://go.microsoft.com/fwlink/?LinkID=155158) ( <https://go.microsoft.com/fwlink/?LinkID=155158> ).                                                                                                                                                                                                    |
|     [/Policy: {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Excluir}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/Value: \<Value> ]      | Especifica o valor do cliente que corresponde a **/FilterType**. Você pode especificar vários valores para um único tipo. Consulte a lista a seguir para obter os valores válidos para **ChassisType**. Para obter informações sobre como obter os valores para todos os outros tipos de filtro, consulte [filtros de grupo de driver](https://go.microsoft.com/fwlink/?LinkID=155158) ( <https://go.microsoft.com/fwlink/?LinkID=155158> ).</br>**Outras**</br>**UnknownChassis**</br>**Área de Trabalho**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Torre**</br>**Portáteis**</br>**Laptop**</br>**Notebook**</br>**Aparelhos**</br>**DockingStation**</br>**AllInOne**</br>**Subnotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Subchassi**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

## <a name="examples"></a>Exemplos

Para adicionar um filtro a um grupo de drivers, digite um dos seguintes:
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

