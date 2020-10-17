---
title: tree
description: Artigo de referência para a árvore, que exibe a estrutura de diretório de um caminho, ou do disco em uma unidade, graficamente.
ms.topic: reference
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c9f586dbcf9072feffe71e1c3d792f10afc951a4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156401"
---
# <a name="tree"></a>tree

Exibe a estrutura de diretório de um caminho ou do disco em uma unidade graficamente. A estrutura exibida por esse comando depende dos parâmetros que você especificar no prompt de comando. Se você não especificar uma unidade ou um caminho, esse comando exibirá a estrutura de árvore começando com o diretório atual da unidade atual.

## <a name="syntax"></a>Sintaxe

```
tree [<drive>:][<path>] [/f] [/a]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<drive>:` | Especifica a unidade que contém o disco para o qual você deseja exibir a estrutura do diretório. |
| `<path>` | Especifica o diretório para o qual você deseja exibir a estrutura de diretório. |
| /f | Exibe os nomes dos arquivos em cada diretório. |
| /a | Especifica o uso de caracteres de texto em vez de caracteres gráficos para mostrar as linhas que vinculam subdiretórios. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para exibir os nomes de todos os subdiretórios no disco na unidade atual, digite:

```
tree \
```

Para exibir uma tela de cada vez, os arquivos em todos os diretórios na unidade C, digite:

```
tree c:\ /f | more
```

Para imprimir uma lista de todos os diretórios na unidade C, digite:

```
tree c:\ /f  prn
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
