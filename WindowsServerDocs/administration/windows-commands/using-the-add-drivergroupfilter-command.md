---
title: Add-DriverGroupFilter
description: Tópico de referência para Add-DriverGroupFilter, que adiciona um filtro a um grupo de drivers em um servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45ed82940e89f4ab760fda4eed640cd62056daf8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721124"
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
| /DriverGroup:\<nome do grupo> |                                                                                                                                                                                                                                                                                                                                                                                                                                                              Especifica o nome do grupo de drivers.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|  [/Server:\<nome do servidor>]  |                                                                                                                                                                                                                                                                                                                                                                                                               Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                                                                                                                                                                                                                                                                                                                               |
| /FilterType:\<filtertype>  |                                                                                                                                                                                                   Especifica o tipo de filtro a ser adicionado ao grupo. Você pode especificar vários tipos de filtro em um único comando. Cada tipo de filtro deve ser seguido por **/Policy** e incluir pelo menos um **/Value**. \<O FilterType> pode ser **BiosVendor**, **biosversion**, **ChassisType**, **manufacturer**, **UUID**, **OsVersion**, **OsEdition**ou **OsLanguage**. Para obter informações sobre como obter os valores para todos os outros tipos de filtro, consulte<https://go.microsoft.com/fwlink/?LinkID=155158>filtros de grupo de [Driver](https://go.microsoft.com/fwlink/?LinkID=155158) ().                                                                                                                                                                                                    |
|     [/Policy: {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Excluir}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/Value:\<valor>]      | Especifica o valor do cliente que corresponde a **/FilterType**. Você pode especificar vários valores para um único tipo. Consulte a lista a seguir para obter os valores válidos para **ChassisType**. Para obter informações sobre como obter os valores para todos os outros tipos de filtro, consulte<https://go.microsoft.com/fwlink/?LinkID=155158>filtros de grupo de [Driver](https://go.microsoft.com/fwlink/?LinkID=155158) ().</br>**Outros**</br>**UnknownChassis**</br>**Área de Trabalho**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Torre**</br>**Portáteis**</br>**Laptop**</br>**Notebook**</br>**Aparelhos**</br>**DockingStation**</br>**AllInOne**</br>**Subnotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Subchassi**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

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

