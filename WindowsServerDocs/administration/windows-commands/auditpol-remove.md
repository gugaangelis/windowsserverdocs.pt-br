---
title: Remover Auditpol
description: Tópico de comandos do Windows para **auditpol remover** -remove a política de auditoria por usuário para uma conta especificada ou todas as contas.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 566827e93d9f8c9dc0d00f4f704513369fbb44ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818297"
---
# <a name="auditpol-remove"></a>Remover Auditpol

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove a política de auditoria por usuário para uma conta especificada ou todas as contas.

## <a name="syntax"></a>Sintaxe
```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/user|Especifica o identificador de segurança (SID) ou nome de usuário para o usuário para os quais a política de auditoria cada usuário deve ser excluído.|
|/allusers|Remove a política de auditoria por usuário para todos os usuários.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
Para remover operações para a política por usuário, você deve ter permissão de gravação ou controle total naquele objeto definido no descritor de segurança. Você também pode executar operações de remoção que possui o **gerenciar o log de auditoria e segurança** direito de usuário (SeSecurityPrivilege). No entanto, esse direito permite acesso adicional que não é necessário para executar a operação de remoção.
## <a name="BKMK_examples"></a>Exemplos
Para remover a política de auditoria por usuário para usuário mikedan por nome, digite:
```
auditpol /remove /user:mikedan
```
Para remover a política de auditoria por usuário para usuário mikedan pelo SID, digite:
```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```
Para remover a política de auditoria por usuário para todos os usuários, digite:
```
auditpol /remove /allusers
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
