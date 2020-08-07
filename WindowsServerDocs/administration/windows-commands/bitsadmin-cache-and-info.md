---
title: bitsadmin cache and info
description: Artigo de referência para o comando Bitsadmin cache e info, que despeja uma entrada de cache específica.
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 537c6173718d8c7deb421915b2ef9697472600a2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894796"
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
