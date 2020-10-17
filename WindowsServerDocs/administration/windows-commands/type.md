---
title: tipo
description: Artigo de referência para o comando Type, que exibe o conteúdo de um arquivo de texto.
ms.topic: reference
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.openlocfilehash: 879cd68b4995c65d623d56e97c0680587eda15b4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156367"
---
# <a name="type"></a>tipo

No Shell de comando do Windows, **tipo** é um comando interno que exibe o conteúdo de um arquivo de texto. Use o comando **Type** para exibir um arquivo de texto sem modificá-lo.

No PowerShell, **Type** é um alias interno para o [cmdlet Get-Content](/powershell/module/microsoft.powershell.management/get-content), que também exibe o conteúdo de um arquivo, mas usando uma sintaxe diferente.

## <a name="syntax"></a>Sintaxe

```
type [<drive>:][<path>]<filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `[<drive>:][<path>]<filename>` | Especifica o local e o nome do arquivo ou arquivos que você deseja exibir. Se o `<filename>` contiver espaços, você deverá colocá-lo entre aspas (por exemplo, "nome de arquivo contendo Spaces.txt"). Você também pode adicionar vários nomes de FileName adicionando espaços entre eles. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se você exibir um arquivo binário ou um arquivo criado por um programa, poderá ver caracteres estranhos na tela, incluindo caracteres formfeed e símbolos de sequência de escape. Esses caracteres representam códigos de controle que são usados no arquivo binário. Em geral, evite usar o comando **Type** para exibir arquivos binários.

## <a name="examples"></a>Exemplos

Para exibir o conteúdo de um arquivo chamado *feriado. mar*, digite:

```
type holiday.mar
```

Para exibir o conteúdo de um arquivo demorado chamado *feriado. mar* uma tela por vez, digite:

```
type holiday.mar | more
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
