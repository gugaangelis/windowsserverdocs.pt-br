---
title: bitsadmin setpriority
description: Artigo de referência para o comando Bitsadmin prepriorion, que define a prioridade do trabalho especificado.
ms.topic: reference
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 25bf7026ceef21fb37824ce99f56389941426b79
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630726"
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
