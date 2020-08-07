---
title: bitsadmin setdisplayname
description: Artigo de referência do comando Bitsadmin SetDisplayName, que define o nome de exibição do trabalho especificado.
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac83fee33703555f1a8ba4b65ae8dc4d0f947916
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893166"
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
