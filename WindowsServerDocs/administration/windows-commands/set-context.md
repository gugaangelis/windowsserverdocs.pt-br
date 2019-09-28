---
title: Definir contexto
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 16f71d831f374f495abf2239cb8e694eee69efdf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370981"
---
# <a name="set-contex"></a>Definir contex



Define o contexto para a criação da cópia de sombra. Se usado sem parâmetros, **Definir contexto** exibirá a ajuda no prompt de comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|clientaccessible|Especifica que a cópia de sombra é utilizável por versões cliente do Windows.|
|persistente|Especifica que a cópia de sombra persiste entre a saída do programa, a redefinição ou a reinicialização.|
|volátil|Exclui a cópia de sombra na saída ou na redefinição.|
|NoWriters|Especifica que todos os gravadores são excluídos.|

## <a name="remarks"></a>Comentários

-   O contexto *ClientAccessible* é persistente por padrão.

## <a name="BKMK_examples"></a>Disso

Para impedir que as cópias de sombra sejam excluídas quando você sair do DiskShadow, digite:
```
set context persistent
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)