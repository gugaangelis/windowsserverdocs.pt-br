---
title: bitsadmin getcompletiontime
description: Artigo de referência para o comando getcompletetime Bitsadmin, que recupera a hora em que o trabalho concluiu a transferência de dados.
ms.topic: reference
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 660a28dcfe99bd5801ba2fc3ad909cec78a671ce
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632243"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime

Recupera a hora em que o trabalho concluiu a transferência de dados.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getcompletiontime <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar a hora em que o trabalho chamado *myDownloadJob* concluiu a transferência de dados:

```
bitsadmin /getcompletiontime myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
