---
title: Usando o comando add-DriverGroupFilter
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16962bd87fabd1ee689b962547c2b1aa470283c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817377"
---
# <a name="using-the-add-drivergroupfilter-command"></a>Usando o comando add-DriverGroupFilter



Adiciona um filtro a um grupo de drivers em um servidor.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/DriverGroup:\<Group Name>|Especifica o nome do grupo de drivers.|
|[/Server:\<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou FQDN. Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/FilterType:\<FilterType>|Especifica o tipo de filtro a ser adicionado ao grupo. Você pode especificar vários tipos de filtro em um único comando. Cada tipo de filtro deve ser seguido por **/Policy** e incluir pelo menos um **/valor**. \<FilterType > pode ser **BiosVendor**, **BiosVersion**, **ChassisType**, **fabricante**, **Uuid**, **OsVersion**, **OsEdition**, ou **OsLanguage**. Para obter informações sobre como obter os valores para todos os outros tipos de filtro, consulte [filtros de grupo de Driver](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).|
|[/Policy:{Include | Exclude}]|Se **/Policy** é definido como **Include**, computadores cliente que correspondem ao filtro são permitidas para instalar os drivers nesse grupo. Se **/Policy** é definido como **excluir**, computadores cliente que correspondem ao filtro não tem permissão para instalar os drivers nesse grupo.|
|[/Value:\<Value>]|Especifica o valor do cliente que corresponde ao **/FilterType**. Você pode especificar vários valores para um único tipo. Consulte a lista a seguir para obter valores válidos para **ChassisType**. Para obter informações sobre como obter os valores para todos os outros tipos de filtro, consulte [filtros de grupo de Driver](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).</br>**Outros**</br>**UnknownChassis**</br>**Área de Trabalho**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Tower**</br>**portátil**</br>**Laptop**</br>**Notebook**</br>**Handheld**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca**|

## <a name="BKMK_examples"></a>Exemplos

Para adicionar um filtro a um grupo de drivers, digite um dos seguintes:
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

