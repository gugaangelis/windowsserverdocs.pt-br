---
title: bitsadmin getcompletiontime
description: Tópico de referência para o comando getcompletetime Bitsadmin, que recupera a hora em que o trabalho concluiu a transferência de dados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b3721401e450ae60fb77534f8eb845ff5ac3443
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718117"
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
