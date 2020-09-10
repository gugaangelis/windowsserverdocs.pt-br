---
title: regsvr32
description: Artigo de referência para o comando regsvr32, que registra arquivos. dll como componentes de comando no registro.
ms.topic: reference
ms.assetid: 3345e964-7d3e-42b8-abeb-42ed6edfe2b2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 670953ecb5de087d660c2d3b1b504e7301245b96
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639843"
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
