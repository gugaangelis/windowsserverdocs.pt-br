---
title: remoção de Auditpol
description: Os comandos do Windows tópico para **remoção do Auditpol** – remove a política de auditoria por usuário para uma conta especificada ou todas as contas.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2d652cb3bf7156bc1b1198616c9f6e57f588acd8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382484"
---
# <a name="auditpol-remove"></a>remoção de Auditpol

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
|/|Especifica o identificador de segurança (SID) ou o nome de usuário para o usuário para o qual a política de auditoria por usuário deve ser excluída.|
|/allusers|Remove a política de auditoria por usuário para todos os usuários.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
para remover operações para a política por usuário, você deve ter a permissão gravar ou controle total nesse objeto definido no descritor de segurança. Você também pode executar operações de remoção por meio do direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar a operação de remoção.
## <a name="BKMK_examples"></a>Disso
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
#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
