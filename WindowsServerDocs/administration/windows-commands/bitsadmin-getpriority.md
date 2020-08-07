---
title: bitsadmin getpriority
description: Artigo de referência para o comando Bitsadmin getanteriority, que recupera a prioridade do trabalho especificado.
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 57d51e4a2a34fb5ae1361e864ee932ac314662b2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894037"
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

- **HIGH**

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
