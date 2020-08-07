---
title: bitsadmin takeownership
description: Artigo de referência para o comando Bitsadmin TakeOwnership, que permite que um usuário com privilégios administrativos assuma a propriedade do trabalho especificado.
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7e37b4508681af58ab07458507101647a0906ff
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880979"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

Permite que um usuário com privilégios administrativos assuma a propriedade do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /takeownership <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ---------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para apropriar-se do trabalho chamado *myDownloadJob*:

```
bitsadmin /takeownership myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
