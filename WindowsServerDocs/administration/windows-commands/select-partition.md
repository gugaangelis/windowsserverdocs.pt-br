---
title: select partition
description: Artigo de referência para * * * *-
ms.topic: reference
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ebed1eda02fa2f97516ccd81d89fcabfe21b430c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635481"
---
# <a name="select-partition"></a>select partition

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

seleciona a partição especificada e desloca o foco para ela. Esse comando também pode ser usado para exibir a partição que atualmente tem o foco no disco selecionado.



## <a name="syntax"></a>Sintaxe

```
select partition=<n>
```

### <a name="parameters"></a>Parâmetros

|   Parâmetro    |                                                                                    Descrição                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| particion\=<n> | O número da partição que receberá o foco. Você pode exibir os números de todas as partições no disco selecionado no momento usando o comando **listar partição** no DiskPart. |

## <a name="remarks"></a>Comentários

-   Para poder selecionar uma partição, primeiro você deve selecionar um disco usando o comando **selecionar disco** .

-   Se nenhum número de partição for especificado, esse comando exibirá a partição que atualmente tem o foco no disco selecionado.

-   se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.

-   se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.

## <a name="examples"></a>Exemplos
Para deslocar o foco para a partição 3, digite:

```
select partitition=3
```

Para exibir a partição que atualmente tem o foco no disco selecionado, digite:

```
select partition
```

## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)




