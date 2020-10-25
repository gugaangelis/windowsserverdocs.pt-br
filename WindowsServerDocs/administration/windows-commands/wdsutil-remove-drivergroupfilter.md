---
title: Remove-DriverGroupFilter
description: Artigo de referência para Remove-DriverGroupFilter, que remove uma regra de filtro de um grupo de drivers em um servidor.
ms.topic: reference
ms.assetid: 837bd5d4-c79d-4714-942d-9875bd8e61dc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6fa543d353f425ee41b836ecc97e562d77d9c19d
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524291"
---
# <a name="remove-drivergroupfilter"></a>Remove-DriverGroupFilter



Remove uma regra de filtro de um grupo de drivers em um servidor.

## <a name="syntax"></a>Sintaxe

```
wdsutil /Remove-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/DriverGroup:\<Group Name>|Especifica o nome do grupo de drivers.|
|[/Server:\<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN. Se um nome de servidor não for especificado, o servidor local será usado.|
|[/FilterType: \<FilterType> ]|Especifica o tipo do filtro a ser removido do grupo. \<FilterType> pode ser um dos seguintes:</br>**BiosVendor**</br>**Biosversion**</br>**ChassisType**</br>**Manufacturer**</br>**Personalizado**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|

## <a name="examples"></a>Exemplos

Para remover um filtro, digite um dos seguintes:
```
wdsutil /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer
```
```
wdsutil /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /FilterType:OSLanguage
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)