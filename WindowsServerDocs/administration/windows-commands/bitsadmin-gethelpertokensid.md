---
title: bitsadmin gethelpertokensid
description: Artigo de referência para o comando Bitsadmin gethelpertokensid, que retorna o SID de um token auxiliar do trabalho de transferência de BITS, se um estiver definido.
ms.topic: reference
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 9cfa2c2a10d1f3947374262bbeb9ab21e559cfee
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033544"
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
