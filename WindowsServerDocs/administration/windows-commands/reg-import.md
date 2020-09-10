---
title: reg import
description: Artigo de referência para o comando reg Import, que copia o conteúdo de um arquivo que contém subchaves de registro exportadas, entradas e valores no registro do computador local.
ms.topic: reference
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 49f83a20b4f9c9e1e64f6e33f0980757de60d9ec
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627029"
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
