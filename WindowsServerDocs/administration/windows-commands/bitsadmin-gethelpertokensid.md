---
title: bitsadmin gethelpertokensid
description: Artigo de referência para o comando Bitsadmin gethelpertokensid, que retorna o SID de um token auxiliar do trabalho de transferência de BITS, se um estiver definido.
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 742c009d1625fe5ba755367a2a93310627d10550
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894231"
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
