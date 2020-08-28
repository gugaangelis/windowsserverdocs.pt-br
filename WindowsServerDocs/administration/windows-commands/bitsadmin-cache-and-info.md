---
title: bitsadmin cache and info
description: Artigo de referência para o comando Bitsadmin cache e info, que despeja uma entrada de cache específica.
ms.topic: reference
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cce96d25b3968c1f975b6426ce8c25e7420f7bd3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028884"
---
# <a name="bitsadmin-cache-and-info"></a>bitsadmin cache and info

Despeja uma entrada de cache específica.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /cache /info recordID [/verbose]
```

### <a name="parameters"></a>Parâmetros

| Paramreter | Descrição |
| -------------- | -------------- |
| recordID | O GUID associado à entrada de cache. |

## <a name="examples"></a>Exemplos

Para despejar a entrada de cache com o valor recordId de {6511FB02-E195-40A2-B595-E8E2F8F47702}:

```
bitsadmin /cache /info {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de cache Bitsadmin](bitsadmin-cache.md)
