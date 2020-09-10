---
title: bitsadmin setdisplayname
description: Artigo de referência do comando Bitsadmin SetDisplayName, que define o nome de exibição do trabalho especificado.
ms.topic: reference
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 57a72d198b7d262f3a7958920e5d54955f8b6270
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630932"
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
