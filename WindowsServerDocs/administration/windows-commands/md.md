---
title: Md
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2fad89fe4b7e8425064301f6020fefaa5705b25
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839579"
---
# <a name="md"></a>Md



Cria um diretório ou subdiretório.

> [!NOTE]
> Esse comando é o mesmo que o comando **mkdir** .

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|> da unidade de \<:|Especifica a unidade na qual você deseja criar o novo diretório.|
|\<caminho >|Obrigatório. Especifica o nome e o local do novo diretório. O comprimento máximo de qualquer caminho único é determinado pelo sistema de arquivos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Extensões de comando, que são habilitadas por padrão, permitem que você use um único comando **MD** para criar diretórios intermediários em um caminho especificado.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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