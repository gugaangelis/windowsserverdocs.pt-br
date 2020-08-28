---
title: type
description: Artigo de referência para o tipo, que exibe o conteúdo de um arquivo de texto.
ms.topic: reference
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: a7e188ec816b4ad16d86018bf7fe92954e778cbf
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036074"
---
# <a name="type"></a>type

No Shell de comando do Windows, **tipo** é um comando interno que exibe o conteúdo de um arquivo de texto. Use o comando **Type** para exibir um arquivo de texto sem modificá-lo.

No PowerShell, o **tipo** é um alias interno para o cmdlet **[Get-Content](/powershell/module/microsoft.powershell.management/get-content)** , que também exibe o conteúdo de um arquivo, mas com uma sintaxe diferente.

## <a name="syntax"></a>Sintaxe

```
type [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName>|Especifica o local e o nome do arquivo ou arquivos que você deseja exibir. Separe vários nomes de arquivo com espaços.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Se o *nome* do arquivo contiver espaços, coloque-o entre aspas (por exemplo, o nome do arquivo que contém Spaces.txt).
-   Se você exibir um arquivo binário ou um arquivo criado por um programa, poderá ver caracteres estranhos na tela, incluindo caracteres formfeed e símbolos de sequência de escape. Esses caracteres representam códigos de controle que são usados no arquivo binário. Em geral, evite usar o comando **Type** para exibir arquivos binários.

## <a name="examples"></a>Exemplos

Para exibir o conteúdo de um arquivo chamado feriado. mar, digite:
```
type holiday.mar
```
Para exibir o conteúdo de um arquivo demorado chamado feriado. mar uma tela por vez, digite:
```
type holiday.mar | more
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
