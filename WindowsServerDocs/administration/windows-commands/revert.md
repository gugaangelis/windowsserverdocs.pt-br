---
title: voltar
description: Artigo de referência para o comando REVERT, que reverte um volume para uma cópia de sombra especificada.
ms.topic: reference
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3c909cec1e503552f68cad55489529585a5eaf51
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640627"
---
# <a name="revert"></a>voltar

Reverte um volume de volta para uma cópia de sombra especificada. Isso tem suporte apenas para cópias de sombra no contexto CLIENTACCESSIBLE. Essas cópias de sombra são persistentes e só podem ser feitas pelo provedor do sistema. Se usado sem parâmetros, **REVERT** exibe a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
revert <shadowcopyID>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<shadowcopyID>` | Especifica a ID da cópia de sombra para a qual reverter o volume. Se você não usar esse parâmetro, o comando exibirá a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
