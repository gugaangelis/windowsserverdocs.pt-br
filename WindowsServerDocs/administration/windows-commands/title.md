---
title: título
description: Artigo de referência do comando title, que cria um título para a janela do prompt de comando.
ms.topic: reference
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f4aaaf198a898589699547c522a734e9e3e5aba2
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156633"
---
# <a name="title"></a>título

Cria um título para a janela de prompt de comando.

## <a name="syntax"></a>Sintaxe

```
title [<string>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<string>` | Especifica o texto a ser exibido como o título da janela do prompt de comando. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Para criar o título da janela para programas do lote, inclua o comando **title** no início de um programa do lote.

- Depois que um título de janela é definido, você pode redefini-lo somente usando o comando **title** .

## <a name="examples"></a>Exemplos

Para alterar o título da janela do prompt de comando para *atualizar os arquivos* enquanto o arquivo em lotes executa o comando de **cópia** e, em seguida, retornar o título de volta ao *prompt de comando*, digite o seguinte script:

```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
