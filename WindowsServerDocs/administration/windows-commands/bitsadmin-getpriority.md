---
title: bitsadmin getpriority
description: Artigo de referência para o comando Bitsadmin getanteriority, que recupera a prioridade do trabalho especificado.
ms.topic: reference
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 2aeff973b0ca285cc8c9852f284e314879f8de02
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028694"
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
