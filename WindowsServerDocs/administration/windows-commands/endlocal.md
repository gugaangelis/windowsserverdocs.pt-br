---
title: endlocal
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e516b2bf9e8a45ada910dfbd93e3ed5e7d86c14
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862137"
---
# <a name="endlocal"></a>endlocal



Termina a localização das alterações de ambiente em um arquivo em lotes e restaura as variáveis de ambiente para seus valores antes do correspondente **setlocal** comando foi executado.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
endlocal
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O **endlocal** comando não tem nenhum efeito fora de um arquivo de script ou lote.
-   Não há implícito **endlocal** comando ao final de um arquivo em lotes.
-   Se as extensões de comando estiverem habilitadas (extensões de comando são habilitadas por padrão), o **endlocal** comando restaura o estado das extensões de comando (ou seja, habilitado ou desabilitado) ao que era antes correspondente  **setlocal** comando foi executado.

> [!NOTE]
> Para obter mais informações sobre como habilitar e desabilitar as extensões de comando, consulte [Cmd](cmd.md).

## <a name="BKMK_examples"></a>Exemplos

Você pode localizar as variáveis de ambiente em um arquivo em lotes. Por exemplo, o programa a seguir inicia o programa de lote superapl na rede, direciona a saída para um arquivo e exibe o arquivo no bloco de notas:
```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)