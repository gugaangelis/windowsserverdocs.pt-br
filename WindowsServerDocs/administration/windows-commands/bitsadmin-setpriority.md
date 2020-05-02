---
title: bitsadmin setpriority
description: Tópico de referência para o comando Bitsadmin setanteriority, que define a prioridade do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 556a1d94700780ea22acc1e4c2f32961c0e43342
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717260"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

Define a prioridade do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setpriority <job> <priority>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| priority | Define a prioridade do trabalho, incluindo:<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |

## <a name="examples"></a>Exemplos

Para definir a prioridade para o trabalho chamado *myDownloadJob* como normal:

```
bitsadmin /setpriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
