---
title: bitsadmin cache and info
description: Tópico de referência para o comando Bitsadmin cache e info, que despeja uma entrada de cache específica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a50e6575a5496ff9f7bcd6a0dc429c7960c6933
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718342"
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
