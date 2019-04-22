---
title: Md
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1396038410ecc5db5a124a1768038c4f8c8bea8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820837"
---
# <a name="md"></a>Md



Cria um diretório ou subdiretório.

> [!NOTE]
> Esse comando é igual a **mkdir** comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive>:|Especifica a unidade na qual você deseja criar o novo diretório.|
|\<Caminho >|Obrigatório. Especifica o nome e o local do novo diretório. O comprimento máximo de um caminho é determinado pelo sistema de arquivos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

As extensões de comando, que são habilitadas por padrão, permitem que você use um único **md** comando para criar diretórios intermediários em um caminho especificado.

## <a name="BKMK_examples"></a>Exemplos

Para criar um diretório chamado arquivo1 dentro do diretório atual, digite:
```
md Directory1
```
Para criar a árvore de diretório Taxes\Property\Current dentro do diretório raiz, com as extensões de comando habilitadas, digite:
```
md \Taxes\Property\Current
```
Para criar a árvore de diretório Taxes\Property\Current dentro do diretório raiz do exemplo anterior, mas com as extensões de comando desabilitadas, digite a seguinte sequência de comandos:
```
md \Taxes
cd \Taxes 
md Property
cd Property
md Current
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

[Cmd](cmd.md)