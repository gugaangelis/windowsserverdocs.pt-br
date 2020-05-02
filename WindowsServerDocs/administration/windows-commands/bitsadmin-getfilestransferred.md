---
title: bitsadmin getfilestransferred
description: Tópico de referência para o comando Bitsadmin getfilestransferred, que recupera o número de arquivos transferidos para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed11739029338ecce5fc4efbe1918873a7f37f62
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717913"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred

Recupera o número de arquivos transferidos para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getfilestransferred <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o número de arquivos transferidos no trabalho chamado *myDownloadJob*:

```
bitsadmin /getfilestransferred myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
