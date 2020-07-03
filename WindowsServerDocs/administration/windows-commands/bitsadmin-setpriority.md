---
title: bitsadmin setpriority
description: Artigo de referência para o comando Bitsadmin prepriorion, que define a prioridade do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 07afd9c636a5dbcd4e70de71b3a6f515e7e02bae
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927591"
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
