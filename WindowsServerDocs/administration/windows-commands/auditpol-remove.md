---
title: auditpol remove
description: Artigo de referência para o comando de remoção de Auditpol, que remove a política de auditoria por usuário para uma conta especificada ou todas as contas.
ms.topic: reference
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a86d5e76daa7023b73760347fb5656fa13ca6418
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633174"
---
# <a name="auditpol-remove"></a>auditpol remove

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove a política de auditoria por usuário para uma conta especificada ou todas as contas.

Para executar operações de *remoção* na política *por usuário* , você deve ter permissões de **controle total** ou de **gravação** para esse objeto definido no descritor de segurança. Você também pode executar as operações de *remoção* se tiver o direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite acesso adicional que não é necessário para executar as operações de *remoção* geral.

## <a name="syntax"></a>Sintaxe

```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| / | Especifica o identificador de segurança (SID) ou o nome de usuário para o usuário para o qual a política de auditoria por usuário deve ser excluída. |
| /allusers | Remove a política de auditoria por usuário para todos os usuários. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para remover a política de auditoria por usuário para Mikedan de usuário por nome, digite:

```
auditpol /remove /user:mikedan
```

Para remover a política de auditoria por usuário para Mikedan de usuário por SID, digite:

```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```

Para remover a política de auditoria por usuário para todos os usuários, digite:

```
auditpol /remove /allusers
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comandos Auditpol](auditpol.md)
