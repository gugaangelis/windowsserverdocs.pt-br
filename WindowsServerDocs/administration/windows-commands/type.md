---
title: type
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 37f66d54983c002d5d09db5cb255d01635a534de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392325"
---
# <a name="type"></a>type


No Shell de comando do Windows, **tipo** é um comando interno que exibe o conteúdo de um arquivo de texto. Use o comando **Type** para exibir um arquivo de texto sem modificá-lo.


No PowerShell, o **tipo** é um alias interno para o cmdlet **[Get-Content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** , que também exibe o conteúdo de um arquivo, mas com uma sintaxe diferente.


Para obter exemplos de como usar esse comando no Shell de comando do Windows (cmd. exe), consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
type [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive >:] [\<Path >] \<FileName >|Especifica o local e o nome do arquivo ou arquivos que você deseja exibir. Separe vários nomes de arquivo com espaços.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Se o *nome* do arquivo contiver espaços, coloque-o entre aspas (por exemplo, "nome do arquivo que contém espaços. txt").
-   Se você exibir um arquivo binário ou um arquivo criado por um programa, poderá ver caracteres estranhos na tela, incluindo caracteres formfeed e símbolos de sequência de escape. Esses caracteres representam códigos de controle que são usados no arquivo binário. Em geral, evite usar o comando **Type** para exibir arquivos binários.

## <a name="BKMK_examples"></a>Disso

Para exibir o conteúdo de um arquivo chamado feriado. mar, digite:
```
type holiday.mar 
```
Para exibir o conteúdo de um arquivo demorado chamado feriado. mar uma tela por vez, digite:
```
type holiday.mar | more 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
