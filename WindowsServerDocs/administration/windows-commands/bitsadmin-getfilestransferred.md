---
title: bitsadmin getfilestransferred
description: Artigo de referência para o comando Bitsadmin getfilestransferred, que recupera o número de arquivos transferidos para o trabalho especificado.
ms.topic: reference
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b4a34e799b2d7e41373a1b4682c77e3ad93d36c2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632095"
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
