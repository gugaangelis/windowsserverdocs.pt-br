---
title: bitsadmin cache and info
description: Artigo de referência para o comando Bitsadmin cache e info, que despeja uma entrada de cache específica.
ms.topic: reference
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 16eda9ddd04a270fbfd754186fd5c9f4a6152cd5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632556"
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
