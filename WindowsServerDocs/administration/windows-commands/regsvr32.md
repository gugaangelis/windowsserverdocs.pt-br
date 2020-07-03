---
title: regsvr32
description: Artigo de referência para o comando regsvr32, que registra arquivos. dll como componentes de comando no registro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7a1a9247b66e5eb1a23c1f5ef33fbcb98c53bd7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930968"
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
