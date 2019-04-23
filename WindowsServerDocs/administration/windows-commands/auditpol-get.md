---
title: auditpol get
description: Tópico de comandos do Windows para **auditpol obter** -recupera a política do sistema, a política por usuário, auditoria de opções e o objeto de descritor de segurança de auditoria.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe13de4e-836c-4207-b47c-64b6272d6c41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83aa1d9d193db977dfe3d375476106d7d6f81e85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881537"
---
# <a name="auditpol-get"></a>auditpol get

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera a política do sistema, a política por usuário, auditoria de opções e o objeto de descritor de segurança de auditoria.

## <a name="syntax"></a>Sintaxe
```
auditpol /get 
[/user[:<username>|<{sid}>]]
[/category:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/subcategory:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/option:<option name>]
[/sd]
[/r]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/user|Exibe a entidade de segurança para os quais a política de auditoria por usuário é consultada. Parâmetro /category ou /subcategory deve ser especificado. O usuário pode ser especificado como um identificador de segurança (SID) ou o nome. Se nenhuma conta de usuário for especificada, a política de auditoria do sistema é consultada.|
|/category|Uma ou mais categorias de auditoria especificadas por nome ou identificador global exclusivo (GUID). Um asterisco (*) pode ser usado para indicar que todas as categorias de auditoria devem ser consultadas.|
|/subcategory|Um ou mais subcategorias de auditoria especificadas por nome ou GUID.|
|/sd|Recupera o descritor de segurança usado para delegar acesso para a política de auditoria.|
|/option|Recupera a política existente para as opções CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects ou AuditBasedirectories.|
|/r|Exibe a saída no formato de relatório, valores separados por vírgulas (CSV).|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
Todas as categorias e subcategorias podem ser especificadas pelo GUID ou nome entre aspas. Os usuários podem ser especificados por nome ou SID.
todas as operações get para a política por usuário e a diretiva do sistema, você deve ter permissão de leitura naquele objeto definido no descritor de segurança. Você também pode executar operações get que possui o **gerenciar o log de auditoria e segurança** direito de usuário (SeSecurityPrivilege). No entanto, esse direito permite acesso adicional que não é necessário para executar a operação get.
## <a name="BKMK_examples"></a>Exemplos
### <a name="examples-for-the-per-user-audit-policy"></a>Exemplos para a política de auditoria por usuário
Para recuperar a política de auditoria por usuário para a conta de convidado e exibir a saída para o sistema de controle detalhadas e categorias de acesso a objeto, digite:
```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:"System","detailed Tracking","Object Access"
```
> [!NOTE]
> Esse comando é útil em dois cenários. Ao monitorar uma conta de usuário específica para a atividade suspeita, você pode usar o comando /get para recuperar os resultados em categorias específicas usando uma diretiva de inclusão para habilitar a auditoria adicionais. Ou, se as configurações de auditoria em uma conta estão fazendo vários, mas eventos supérfluos, você pode usar o comando /get para filtrar eventos estranhos para essa conta com uma política de exclusão. Para obter uma lista de todas as categorias, use o comando de /category auditpol /list.
Para recuperar a política de auditoria por usuário para uma categoria e uma subcategoria, que reporta as configurações inclusivas e exclusivas para essa subcategoria sob a categoria de sistema para a conta de convidado, digite:
```
auditpol /get /user:guest /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```
Para exibir a saída no formato de relatório e incluir o nome do computador, o destino da política, subcategoria, subcategoria GUID, configurações de inclusão e configurações de exclusão, digite:
```
auditpol /get /user:guest /category:detailed Tracking" /r
```
### <a name="examples-for-the-system-audit-policy"></a>Exemplos para a política de auditoria do sistema
Para recuperar a política para o sistema de categorias e subcategorias, que reporta as configurações de política de categoria e subcategoria para a política de auditoria do sistema, digite:
```
auditpol /get /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```
Para recuperar a política para a categoria de rastreamento detalhada e as subcategorias em formato de relatório e incluir o nome do computador, o destino da política, a subcategoria, a subcategoria GUID, configurações de inclusão e configurações de exclusão, digite:
```
auditpol /get /category:"detailed Tracking" /r
```
Para recuperar a política para duas categorias com as categorias especificadas como GUIDs, que relata todas as configurações de diretiva de auditoria de todas as subcategorias em duas categorias, tipo:
```
auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```
### <a name="examples-for-auditing-options"></a>Exemplos de opções de auditoria
Para recuperar o estado, habilitado ou desabilitado, da opção AuditBaseObjects, digite:
```
auditpol /get /option:AuditBaseObjects
```
> [!NOTE]
> As opções disponíveis são AuditBaseObjects, AuditBaseOperations e FullprivilegeAuditing.
Para recuperar o estado habilitado, desabilitado ou 2 da opção CrashOnAuditFail, digite:
```
auditpol /get /option:CrashOnAuditFail /r
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
