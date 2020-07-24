---
title: bitsadmin sethelpertoken
description: Artigo de referência para o comando Bitsadmin sethelpertoken, que define o token primário do prompt de comando atual (ou um token de conta de usuário local arbitrário, se especificado) como um token auxiliar do trabalho de transferência de BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: e9fe469b8ae4a8a553245b1e22d48ee2cacf23e3
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955698"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

Define o token primário do prompt de comando atual (ou um token de conta de usuário local arbitrário, se especificado) como um [token auxiliar](/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)do trabalho de transferência de bits.

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
