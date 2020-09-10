---
title: bitsadmin getpriority
description: Artigo de referência para o comando Bitsadmin getanteriority, que recupera a prioridade do trabalho especificado.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 397a762a210aeae7a02e49283330a2d4214876e6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631762"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin getpriority

Recupera a prioridade do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

#### <a name="output"></a>Saída

A prioridade retornada para esse comando pode ser:

- **FRENTE**

- **ELEVADA**

- **NORMAL**

- **PEQUENA**

- **UNKNOWN**

## <a name="examples"></a>Exemplos

Para recuperar a prioridade para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
