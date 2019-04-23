---
title: type
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 4ceb7365d34a2aeca21d1a699730a589f98fd549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887397"
---
# <a name="type"></a>type


No shell de comando do Windows, **tipo** é um comando que exibe o conteúdo de um arquivo de texto. Use o **tipo** comando para exibir um arquivo de texto sem modificá-lo.


No PowerShell, **tipo** é um alias interno para o **[Get-Content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** cmdlet que também exibe o conteúdo de um arquivo, mas com uma sintaxe diferente.


Para obter exemplos de como usar esse comando no shell de comando do Windows (Cmd.exe), consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
type [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName>|Especifica o local e o nome do arquivo ou arquivos que você deseja exibir. Separe vários nomes de arquivos com espaços.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Se *FileName* contiver espaços, coloque-o entre aspas (por exemplo, "arquivo nome contendo Spaces.txt").
-   Se você exibir um arquivo binário ou um arquivo que é criado por um programa, você poderá ver os caracteres estranhos na tela, incluindo caracteres de tabulação e símbolos de sequência de escape. Esses caracteres representam códigos de controle que são usados no arquivo binário. Em geral, evite usar o **tipo** comando para exibir arquivos binários.

## <a name="BKMK_examples"></a>Exemplos

Para exibir o conteúdo de um arquivo denominado Feriado, digite:
```
type holiday.mar 
```
Para exibir o conteúdo de um arquivo longo chamado maiores informações sobre uma tela por vez, digite:
```
type holiday.mar | more 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
