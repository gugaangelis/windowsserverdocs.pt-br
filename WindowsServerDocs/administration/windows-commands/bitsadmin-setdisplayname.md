---
title: bitsadmin setdisplayname
description: Artigo de referência do comando Bitsadmin SetDisplayName, que define o nome de exibição do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7cd1ce068e1e2a89b27ee88653fdd014d2da178
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927796"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname

Define o nome de exibição para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setdisplayname <job> <display_name>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| display_name | Texto usado como o nome exibido para o trabalho específico. |

## <a name="examples"></a>Exemplos

Para definir o nome de exibição do trabalho como *myDownloadJob*:

```
bitsadmin /setdisplayname myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
