---
title: remoção de Auditpol
description: O tópico de comandos do Windows para **remoção de Auditpol**, que remove a política de auditoria por usuário para uma conta especificada ou todas as contas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1eda43d6708a31b2966022d2ae2c162bbfc888cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851169"
---
# <a name="auditpol-remove"></a>remoção de Auditpol

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove a política de auditoria por usuário para uma conta especificada ou todas as contas.

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

## <a name="remarks"></a>Comentários

Para remover operações para a política por usuário, você deve ter a permissão gravar ou controle total nesse objeto definido no descritor de segurança. Você também pode executar operações de remoção por meio do direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar a operação de remoção.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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
