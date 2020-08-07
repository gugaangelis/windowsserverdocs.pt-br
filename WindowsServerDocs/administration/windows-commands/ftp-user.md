---
title: ftp user
description: Artigo de referência para o comando de usuário de FTP, que especifica um usuário para o computador remoto.
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd015b7f84a6f5a4f3ee10a3cbe351a5bfa4a563
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888818"
---
# <a name="ftp-user"></a>ftp user

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Especifica um usuário para o computador remoto.

## <a name="syntax"></a>Sintaxe

```
user <username> [<password>] [<account>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<username>` | Especifica um nome de usuário com o qual fazer logon no computador remoto. |
| `[<password>]` | Especifica a senha para o *nome de usuário*. Se uma senha não for especificada, mas for necessária, o comando **FTP** solicitará a senha. |
| `[<account>]` | Especifica uma conta com a qual fazer logon no computador remoto. Se uma *conta* não for especificada, mas for necessária, o comando **FTP** solicitará a conta. |

### <a name="examples"></a>Exemplos

Para especificar *user1* com a senha *password1*, digite:

```
user User1 Password1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
