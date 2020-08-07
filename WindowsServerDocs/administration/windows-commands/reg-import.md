---
title: reg import
description: Artigo de referência para o comando reg Import, que copia o conteúdo de um arquivo que contém subchaves de registro exportadas, entradas e valores no registro do computador local.
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4461f10cb31447a40f3d49df7731980f0f8a88a2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884140"
---
# <a name="reg-import"></a>reg import

Copia o conteúdo de um arquivo que contém subchaves de registro exportadas, entradas e valores no registro do computador local.

## <a name="syntax"></a>Sintaxe

```
reg import <filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<filename>` | Especifica o nome e o caminho do arquivo que tem o conteúdo a ser copiado no registro do computador local. Esse arquivo deve ser criado com antecedência usando **reg export**. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os valores de retorno para a operação de **importação de reg** são:

    | Valor | Descrição |
    |--|--|
    | 0 | Êxito |
    | 1 | Falha |

### <a name="examples"></a>Exemplos

Para importar entradas do registro do arquivo chamado AppBkUp. reg, digite:

```
reg import AppBkUp.reg
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando reg export](reg-export.md)
