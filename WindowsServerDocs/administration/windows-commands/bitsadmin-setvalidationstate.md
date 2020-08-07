---
title: bitsadmin setvalidationstate
description: Artigo de referência do comando setvalidable Bitsadmin, que define o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dcdbd017f225704fc20d0472346d98fd84bb2c0
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881026"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate

Define o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setvalidationstate <job> <file_index> <TRUE|FALSE>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ---------- |
| Trabalho | O nome de exibição ou o GUID do trabalho. |
| file_index | Começa em 0. |
| VERDADEIRO ou falso | **True** ativa a validação de conteúdo para o arquivo especificado, enquanto **false** o desativa. |

## <a name="examples"></a>Exemplos

Para definir o estado de validação de conteúdo do arquivo 2 como TRUE para o trabalho chamado *myDownloadJob*:

```
bitsadmin /setvalidationstate myDownloadJob 2 TRUE
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
