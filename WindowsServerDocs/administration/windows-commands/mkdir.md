---
title: mkdir
description: Artigo de referência para o comando mkdir, que cria um diretório ou subdiretório.
ms.topic: reference
ms.assetid: 033a57a2-5deb-4c98-aa78-61ce8df2a330
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bfefe4bc040d3e3f28adb6b37529764f28fca25b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639505"
---
# <a name="mkdir"></a>mkdir

Cria um diretório ou subdiretório. Extensões de comando, que são habilitadas por padrão, permitem que você use um único comando **mkdir** para criar diretórios intermediários em um caminho especificado.

> [!NOTE]
> Esse comando é o mesmo que o [comando MD](md.md).

## <a name="syntax"></a>Sintaxe

```
mkdir [<drive>:]<path>
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
mkdir Directory1
```

Para criar a árvore de diretórios *Taxes\Property\Current* no diretório raiz, com extensões de comando habilitadas, digite:

```
mkdir \Taxes\Property\Current
```

Para criar a árvore de diretórios *Taxes\Property\Current* dentro do diretório raiz como no exemplo anterior, mas com as extensões de comando desabilitadas, digite a seguinte sequência de comandos:

```
mkdir \Taxes
mkdir \Taxes\Property
mkdir \Taxes\Property\Current
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando MD](md.md)