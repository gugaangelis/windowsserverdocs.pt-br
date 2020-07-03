---
title: bitsadmin setvalidationstate
description: Artigo de referência do comando setvalidable Bitsadmin, que define o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae776279b742e3af5af3cf555765007bbf0eb8de
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927492"
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
