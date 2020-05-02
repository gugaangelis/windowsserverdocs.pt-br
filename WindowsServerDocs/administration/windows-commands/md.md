---
title: Md
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a605571fb74af99d0f365a100dd33fd4db0d3f22
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724001"
---
# <a name="md"></a>Md



Cria um diretório ou subdiretório.

> [!NOTE]
> Esse comando é o mesmo que o comando **mkdir** .



## <a name="syntax"></a>Sintaxe

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> da unidade:|Especifica a unidade na qual você deseja criar o novo diretório.|
|\<Caminho>|Obrigatórios. Especifica o nome e o local do novo diretório. O comprimento máximo de qualquer caminho único é determinado pelo sistema de arquivos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Extensões de comando, que são habilitadas por padrão, permitem que você use um único comando **MD** para criar diretórios intermediários em um caminho especificado.

## <a name="examples"></a>Exemplos

Para criar um diretório chamado directory1 no diretório atual, digite:
```
md Directory1
```
Para criar a árvore de diretórios Taxes\Property\Current no diretório raiz, com extensões de comando habilitadas, digite:
```
md \Taxes\Property\Current
```
Para criar a árvore de diretórios Taxes\Property\Current dentro do diretório raiz como no exemplo anterior, mas com as extensões de comando desabilitadas, digite a seguinte sequência de comandos:
```
md \Taxes
md \Taxes\Property
md \Taxes\Property\Current
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Cmd](cmd.md)