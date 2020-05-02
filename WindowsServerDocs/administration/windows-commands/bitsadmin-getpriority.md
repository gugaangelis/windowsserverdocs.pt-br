---
title: bitsadmin getpriority
description: Tópico de referência para o comando Bitsadmin getanteriority, que recupera a prioridade do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 38f92e83ccf5b048d168ce6a21c6026f490b18bf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717672"
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

- **LOW**

- **CONHECIDOS**

## <a name="examples"></a>Exemplos

Para recuperar a prioridade para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
