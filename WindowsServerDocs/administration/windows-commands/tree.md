---
title: tree
description: Artigo de referência para a árvore, que exibe a estrutura de diretório de um caminho, ou do disco em uma unidade, graficamente.
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44e3e54f986cc4bd4459d4e007c5111b664a6a45
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87897094"
---
# <a name="tree"></a>tree

Exibe a estrutura de diretório de um caminho ou do disco em uma unidade graficamente.



## <a name="syntax"></a>Sintaxe

```
tree [<Drive>:][<Path>] [/f] [/a]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive>:|Especifica a unidade que contém o disco para o qual você deseja exibir a estrutura do diretório.|
|\<Path>|Especifica o diretório para o qual você deseja exibir a estrutura de diretório.|
|/f|Exibe os nomes dos arquivos em cada diretório.|
|/a|Especifica que a **árvore** deve usar caracteres de texto em vez de caracteres gráficos para mostrar as linhas que vinculam subdiretórios.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

A estrutura exibida pela **árvore** depende dos parâmetros que você especificar no prompt de comando. Se você não especificar uma unidade ou caminho, a **árvore** exibirá a estrutura de árvore que começa com o diretório atual da unidade atual.

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