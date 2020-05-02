---
title: bitsadmin list
description: Tópico de referência para o comando lista de Bitsadmin, que lista os trabalhos de transferência de Propriedade do usuário atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 262589293a147cc1bae98da8fdca047c5f914094
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717429"
---
# <a name="bitsadmin-list"></a>bitsadmin list

Lista os trabalhos de transferência de Propriedade do usuário atual.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /list [/allusers][/verbose]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| /allusers | Opcional. Lista os trabalhos para todos os usuários. Você deve ter privilégios de administrador para usar esse parâmetro. |
| /verbose | Opcional. Fornece informações detalhadas sobre cada trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar informações sobre trabalhos pertencentes ao usuário atual.

```
bitsadmin /list
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
