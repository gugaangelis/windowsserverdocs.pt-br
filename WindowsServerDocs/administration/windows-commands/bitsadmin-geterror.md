---
title: bitsadmin geterror
description: Artigo de referência para o comando GetError Bitsadmin, que recupera informações de erro detalhadas para o trabalho especificado.
ms.topic: reference
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1cfa4c5a7d0899e2d5eb34fd089944f2b801993a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632127"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror

Recupera informações detalhadas de erro para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /geterror <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar as informações de erro para o trabalho chamado *myDownloadJob*:

```
bitsadmin /geterror myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
