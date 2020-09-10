---
title: ftp user
description: Artigo de referência para o comando de usuário de FTP, que especifica um usuário para o computador remoto.
ms.topic: reference
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5371a5f2ee731ceccfdc484c5473998338b6d37c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621559"
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
