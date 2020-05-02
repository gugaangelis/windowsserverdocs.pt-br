---
title: Definir contexto
description: Tópico de referência para o contexto de conjunto, que define o contexto para a criação da cópia de sombra.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9494cb8a0a6b0e320240d74980049a4e49843ecd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721928"
---
# <a name="set-contex"></a>Definir contex

Define o contexto para a criação da cópia de sombra. Se usado sem parâmetros, **Definir contexto** exibirá a ajuda no prompt de comando.



## <a name="syntax"></a>Sintaxe

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|clientaccessible|Especifica que a cópia de sombra é utilizável por versões cliente do Windows.|
|persistente|Especifica que a cópia de sombra persiste entre a saída do programa, a redefinição ou a reinicialização.|
|volatile|Exclui a cópia de sombra na saída ou na redefinição.|
|NoWriters|Especifica que todos os gravadores são excluídos.|

## <a name="remarks"></a>Comentários

-   O contexto *ClientAccessible* é persistente por padrão.

## <a name="examples"></a>Exemplos

Para impedir que as cópias de sombra sejam excluídas quando você sair do DiskShadow, digite:
```
set context persistent
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)