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
ms.openlocfilehash: 21dee45610823cd70e8b7209ec99e080746316f9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927788"
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
