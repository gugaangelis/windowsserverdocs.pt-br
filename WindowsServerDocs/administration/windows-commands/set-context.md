---
title: Definir contexto
description: Artigo de referência para o comando Set Context, que define o contexto para a criação da cópia de sombra.
ms.topic: reference
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ce1414ab90f116f58618fcd67bc203f25dc58e46
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389090"
---
# <a name="set-context"></a>Definir contexto

Define o contexto para a criação da cópia de sombra. Se usado sem parâmetros, **Definir contexto** exibirá a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| clientaccessible | Especifica que a cópia de sombra é utilizável por versões cliente do Windows. Esse contexto é persistente por padrão. |
| persistente | Especifica que a cópia de sombra persiste entre a saída do programa, a redefinição ou a reinicialização. |
| volatile | Exclui a cópia de sombra na saída ou na redefinição. |
| NoWriters | Especifica que todos os gravadores são excluídos. |

## <a name="examples"></a>Exemplos

Para impedir que as cópias de sombra sejam excluídas quando você sair do DiskShadow, digite:

```
set context persistent
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Set Metadata](set-metadata.md)

- [comando set option](set-option.md)

- [Definir comando detalhado](set-verbose.md)
