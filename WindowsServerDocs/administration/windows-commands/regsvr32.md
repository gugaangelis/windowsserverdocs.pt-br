---
title: regsvr32
description: Artigo de referência para o comando regsvr32, que registra arquivos. dll como componentes de comando no registro.
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a591813480163a6d43329222d20e2a29651f576
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883886"
---
# <a name="regsvr32"></a>regsvr32

Registra arquivos. dll como componentes de comando no registro.

## <a name="syntax"></a>Sintaxe

```
regsvr32 [/u] [/s] [/n] [/i[:cmdline]] <Dllname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /u | Cancela o registro do servidor. |
| /s | Impede a exibição de mensagens. |
| /n | Impede a chamada de **DllRegisterServer**. Esse parâmetro requer que você também use o parâmetro **/i** . |
| /i`<cmdline>` | Passa uma cadeia de caracteres de linha de comando opcional (*cmdline*) para **DllInstall**. Se você usar esse parâmetro com o parâmetro **/u** , ele chamará **DllUninstall**. |
| `<Dllname>` | O nome do arquivo. dll que será registrado. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para registrar o. dll para o esquema de Active Directory, digite:

```
regsvr32 schmmgmt.dll
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
