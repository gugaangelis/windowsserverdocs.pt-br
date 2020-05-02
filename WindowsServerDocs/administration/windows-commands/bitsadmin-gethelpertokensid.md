---
title: bitsadmin gethelpertokensid
description: Tópico de referência para o comando Bitsadmin gethelpertokensid, que retorna o SID de um token auxiliar do trabalho de transferência de BITS, se um estiver definido.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: c45bf86d8a7364289db41fa390f319270a2a8386
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717900"
---
# <a name="bitsadmin-gethelpertokensid"></a>bitsadmin gethelpertokensid

Retorna o SID de um [token auxiliar](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)do trabalho de transferência de bits, se um estiver definido.

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
