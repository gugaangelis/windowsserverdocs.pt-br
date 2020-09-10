---
title: bitsadmin gethelpertokensid
description: Artigo de referência para o comando Bitsadmin gethelpertokensid, que retorna o SID de um token auxiliar do trabalho de transferência de BITS, se um estiver definido.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 645957ab5526c98e806a4e7875e1baece3d375b3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632048"
---
# <a name="bitsadmin-gethelpertokensid"></a>bitsadmin gethelpertokensid

Retorna o SID de um [token auxiliar](/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)do trabalho de transferência de bits, se um estiver definido.

> [!NOTE]
> Esse comando não tem suporte no BITS 3,0 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /gethelpertokensid <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o SID de um trabalho de transferência de BITS chamado *myDownloadJob*:

```
bitsadmin /gethelpertokensid myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
