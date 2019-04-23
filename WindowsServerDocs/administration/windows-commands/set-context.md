---
title: Definir o contexto
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f24e795f2d7c92d462cf822e70e4830b53827e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845847"
---
# <a name="set-contex"></a>Contexto de conjunto



Define o contexto para a criação de cópias de sombra. Se usado sem parâmetros, **definir o contexto de** exibe a Ajuda no prompt de comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|ClientAccessible|Especifica que a cópia de sombra é utilizável por versões cliente do Windows.|
|persistent|Especifica que a cópia de sombra persiste entre a saída do programa, redefinir ou reinicialização.|
|volátil|Exclui a cópia de sombra em sair ou redefinir.|
|NoWriters|Especifica que todos os gravadores são excluídos.|

## <a name="remarks"></a>Comentários

-   O *clientaccessible* contexto é persistente por padrão.

## <a name="BKMK_examples"></a>Exemplos

Para evitar cópias de sombra sejam excluídas quando você sai do DiskShadow, digite:
```
set context persistent
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)