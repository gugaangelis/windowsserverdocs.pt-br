---
title: unexpose
description: Artigo de referência para o comando unexpote, que não expõe uma cópia de sombra exposta.
ms.topic: reference
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 35e545d632790f8c4d7d69d6e9fea1e7e1a07dc4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156317"
---
# <a name="unexpose"></a>unexpose

Não expõe uma cópia de sombra que foi exposta usando o [comando Expo](expose.md). A cópia de sombra exposta pode ser especificada por sua ID de sombra, letra de unidade, compartilhamento ou ponto de montagem.

## <a name="syntax"></a>Sintaxe

```
unexpose {<shadowID> | <drive:> | <share> | <mountpoint>}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<shadowID>` | Exibe a cópia de sombra especificada pela ID de sombra fornecida. Você pode usar um alias existente ou uma variável de ambiente no lugar de `<shadowID>` . Use o [comando Add](add.md) sem parâmetros para ver todos os aliases existentes. |
| `<drive:>` | Exibe a cópia de sombra associada à letra da unidade especificada (por exemplo, a unidade P). |
| `<share>` | Exibe a cópia de sombra associada ao compartilhamento especificado (por exemplo, `\\MachineName` ). |
| `<mountpoint>` | Exibe a cópia de sombra associada ao ponto de montagem especificado (por exemplo, `C:\shadowcopy\` ). |
| add | Usado sem parâmetros mostrará os aliases existentes. |

## <a name="examples"></a>Exemplos

Para desexpor a cópia de sombra associada à * unidade P: \* , digite:

```
unexpose P:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Adicionar comando](add.md)

- [expor comando](expose.md)
