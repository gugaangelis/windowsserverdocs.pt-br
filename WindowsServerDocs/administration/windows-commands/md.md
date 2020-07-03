---
title: md
description: Artigo de referência para o comando MD, que cria um diretório ou subdiretório.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7d5dac14572bfe53f92333cddcdc68bfa0aab1a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922096"
---
# <a name="md"></a>md

Cria um diretório ou subdiretório. Extensões de comando, que são habilitadas por padrão, permitem que você use um único comando **MD** para criar diretórios intermediários em um caminho especificado.

> [!NOTE]
> Esse comando é o mesmo que o [comando mkdir](mkdir.md).

## <a name="syntax"></a>Sintaxe

```
md [<drive>:]<path>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<drive>`: | Especifica a unidade na qual você deseja criar o novo diretório. |
| `<path>` | Especifica o nome e o local do novo diretório. O comprimento máximo de qualquer caminho único é determinado pelo sistema de arquivos. Esse é um parâmetro necessário. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para criar um diretório chamado *directory1* no diretório atual, digite:

```
md Directory1
```

Para criar a árvore de diretórios *Taxes\Property\Current* no diretório raiz, com extensões de comando habilitadas, digite:

```
md \Taxes\Property\Current
```

Para criar a árvore de diretórios *Taxes\Property\Current* dentro do diretório raiz como no exemplo anterior, mas com as extensões de comando desabilitadas, digite a seguinte sequência de comandos:

```
md \Taxes
md \Taxes\Property
md \Taxes\Property\Current
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando mkdir](mkdir.md)