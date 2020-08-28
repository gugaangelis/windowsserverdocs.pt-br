---
title: bitsadmin getvalidationstate
description: Artigo de referência do comando getvalidable Bitsadmin, que relata o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.
ms.topic: reference
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 173bbaad6508ec3ae8100232fda598fe7a422296
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028634"
---
# <a name="bitsadmin-getvalidationstate"></a>bitsadmin getvalidationstate

Relata o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getvalidationstate <job> <file_index>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| file_index | Começa em 0. |

## <a name="examples"></a>Exemplos

Para recuperar o estado de validação de conteúdo do arquivo 2 no trabalho chamado *myDownloadJob*:

```
bitsadmin /getvalidationstate myDownloadJob 1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
