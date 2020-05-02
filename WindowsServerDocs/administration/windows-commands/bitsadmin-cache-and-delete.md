---
title: bitsadmin cache and delete
description: Tópico de referência para o cache Bitsadmin e o comando delete, que exclui uma entrada de cache específica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62c0c3d5b2cc188e8a8987c7ca502cdeaf932410
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718455"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin cache and delete

Exclui uma entrada de cache específica.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /cache /delete recordID
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| recordID | O GUID associado à entrada de cache. |

## <a name="examples"></a>Exemplos

Para excluir a entrada de cache com recordId de {6511FB02-E195-40A2-B595-E8E2F8F47702}:

```
bitsadmin /cache /delete {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de cache Bitsadmin](bitsadmin-cache.md)
