---
title: bitsadmin sethelpertoken
description: Tópico de referência para o comando Bitsadmin sethelpertoken, que define o token primário do prompt de comando atual (ou um token de conta de usuário local arbitrário, se especificado) como um token auxiliar do trabalho de transferência de BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: b125f95e262c2fd78f20266e3e2b6c80cea5a789
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719400"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

Define o token primário do prompt de comando atual (ou um token de conta de usuário local arbitrário, se especificado) como um [token auxiliar](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)do trabalho de transferência de bits.

> [!NOTE]
> Esse comando não tem suporte no BITS 3,0 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /sethelpertoken <job> [<user_name@domain> <password>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| `<username@domain>` `<password>` | Opcional. As credenciais de conta de usuário local para as quais o token deve ser usado. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
