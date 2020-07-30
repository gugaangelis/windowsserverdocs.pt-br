---
title: rem
description: Artigo de referência do comando REM, que registra os comentários em um script, lote ou config.sys arquivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe0bfce3f9f72d0a32ef5b3bb540e5a297df24a1
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409697"
---
# <a name="rem"></a>rem

Registra comentários em um script, lote ou config.sys arquivo. Se nenhum comentário for especificado, **REM** adicionará espaçamento vertical.

## <a name="syntax"></a>Sintaxe

```
rem [<comment>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<comment>` | Especifica uma cadeia de caracteres a ser incluída como um comentário. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- O comando **REM** não exibe comentários na tela. Para exibir comentários na tela, você deve incluir o comando **Echo on** em seu arquivo.

- Você não pode usar um caractere de redirecionamento ( `<` ou `>` ) ou pipe ( `|` ) em um comentário de arquivo em lotes.

- Embora seja possível usar **REM** sem um comentário para adicionar espaçamento vertical a um arquivo em lotes, você também pode usar linhas em branco. As linhas em branco são ignoradas quando um programa em lotes é processado.

### <a name="examples"></a>Exemplos

Para adicionar espaçamento vertical por meio de comentários de arquivo em lotes, digite:

```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause
format b: /v chkdsk b:
```

Para incluir um comentário explicativo antes do comando **prompt** em um arquivo config.sys, digite:

```
rem Set prompt to indicate current directory
prompt $p$g
```

Para fornecer um comentário sobre o que um script faz, digite:

```
rem The commands in this script set up 3 drives.
rem The first drive is a primary partition and is
rem assigned the letter D. The second and third drives
rem are logical partitions, and are assigned letters
rem E and F.
create partition primary size=2048
assign d:
create partition extended
create partition logical size=2048
assign e:
create partition logical
assign f:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)