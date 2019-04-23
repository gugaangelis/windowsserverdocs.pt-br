---
title: rem
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85c8a69bf21a386cd36e45bbca6dacd35aef2509
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846997"
---
# <a name="rem"></a>rem



Comentários de registros (Observações) em um arquivo em lotes ou no CONFIG. SYS. Se nenhum comentário for especificado, **rem** adiciona espaçamento vertical.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
rem [<Comment>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Comentário >|Especifica uma cadeia de caracteres a ser incluído como um comentário.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O **rem** comando exibe os comentários na tela. Você deve usar o **eco** comando em lote ou no CONFIG. Sys para exibir comentários na tela.
-   Você não pode usar um caractere de redirecionamento (**<** ou **>**) ou pipe (**|**) em um comentário do arquivo em lotes.
-   Embora você possa usar **rem** sem um comentário para adicionar espaçamento vertical para um arquivo em lotes, você também pode usar linhas em branco. Linhas em branco são ignoradas quando um programa em lote é processado.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir mostra um arquivo em lotes que utiliza comentários para comentários em espaçamento vertical:
```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause 
format b: /v chkdsk b: 
```
Para incluir uma explicação antes do **prompt** comando no arquivo CONFIG. SYS arquivo, adicione as seguintes linhas ao arquivo CONFIG. SYS:
```
rem Set prompt to indicate current directory
prompt $p$g
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)