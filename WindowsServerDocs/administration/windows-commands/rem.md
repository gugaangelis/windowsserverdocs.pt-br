---
title: rem
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d115548f15ff45087a771458062da8a3ef919eb3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722459"
---
# <a name="rem"></a>rem



Registra comentários (comentários) em um arquivo em lotes ou em uma configuração. Sistema. Se nenhum comentário for especificado, **REM** adicionará espaçamento vertical.



## <a name="syntax"></a>Sintaxe

```
rem [<Comment>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Comentário>|Especifica uma cadeia de caracteres a ser incluída como um comentário.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O comando **REM** não exibe comentários na tela. Você deve usar o comando **Echo on** em seu lote ou configuração. SYS para exibir comentários na tela.
-   Você não pode usar um caractere de**<** redirecionamento **>**(ou) ou**|** pipe () em um comentário de arquivo em lotes.
-   Embora seja possível usar **REM** sem um comentário para adicionar espaçamento vertical a um arquivo em lotes, você também pode usar linhas em branco. As linhas em branco são ignoradas quando um programa em lotes é processado.

## <a name="examples"></a>Exemplos

Para mostrar um arquivo em lotes que usa comentários para comentários e espaçamento vertical:
```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause 
format b: /v chkdsk b: 
```
Para incluir um comentário explicativo antes do comando **prompt** em sua configuração. SYS, adicione as seguintes linhas a CONFIG. SISTEMA
```
rem Set prompt to indicate current directory
prompt $p$g
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)