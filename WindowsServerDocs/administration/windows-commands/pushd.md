---
title: pushd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 548f39921c1f6aa3837e6443e396922396eb84f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887457"
---
# <a name="pushd"></a>pushd



Armazena o diretório atual para uso pela **popd** comando e, em seguida, alterações no diretório especificado.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
pushd [<Path>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Caminho >|Especifica o diretório para tornar o diretório atual. Este comando dá suporte a caminhos relativos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Toda vez que usar o **pushd** de comando, um único diretório é armazenado para seu uso. No entanto, você pode armazenar vários diretórios usando o **pushd** comando várias vezes.

    Os diretórios são armazenados em sequência em uma pilha virtual. Se você usar o **pushd** uma vez, o diretório em que você use o comando é colocado na parte inferior da pilha de comando. Se você usar o comando novamente, a segunda pasta será colocada sobre o primeiro deles. O processo se repete toda vez que usar o **pushd** comando.

    Você pode usar o **popd** comando para alterar o diretório atual para o diretório armazenado mais recentemente pelo **pushd** comando. Se você usar o **popd** de comando, o diretório no topo da pilha é removido da pilha e o diretório atual é alterado para esse diretório. Se você usar o **popd** comando novamente, a próxima pasta na pilha é removida.
-   Se as extensões de comando estiverem habilitadas, o **pushd** comando aceita um caminho de rede ou uma letra de unidade e caminho.
-   Se você especificar um caminho de rede, o **pushd** comando temporariamente atribui a letra de unidade não usada de mais alta (começando com z) para o recurso de rede especificado. O comando, em seguida, altera a unidade atual e o diretório para o diretório especificado na unidade recentemente atribuído. Se você usar o **popd** com as extensões de comando habilitadas, o **popd** comando remove a atribuição de letra de unidade criada por **pushd**.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir mostra como você pode usar o **pushd** comando e o **popd** comando em um arquivo em lotes para alterar o diretório atual no qual o arquivo em lotes foi executado e, em seguida, reverter essa alteração:
```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

[Popd](popd.md)