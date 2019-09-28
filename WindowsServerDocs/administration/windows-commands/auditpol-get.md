---
title: Auditpol Get
description: O tópico de comandos do Windows para **Auditpol Get** -recupera a política do sistema, a política por usuário, as opções de auditoria e o objeto do descritor de segurança de auditoria.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 296fc5afb540411d76b563faca42fc045b8df3b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382493"
---
# <a name="auditpol-get"></a>Auditpol Get

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera a política do sistema, a política por usuário, as opções de auditoria e o objeto do descritor de segurança de auditoria.

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

|  Parâmetro   |                                                                                                                                         Descrição                                                                                                                                          |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /     | Exibe a entidade de segurança para a qual a política de auditoria por usuário é consultada. O parâmetro/Category ou/Subcategory deve ser especificado. O usuário pode ser especificado como um SID (identificador de segurança) ou nome. Se nenhuma conta de usuário for especificada, a política de auditoria do sistema será consultada. |
|  /Category   |                                                          Uma ou mais categorias de auditoria especificadas pelo GUID (identificador global exclusivo) ou pelo nome. Um asterisco (\*) pode ser usado para indicar que todas as categorias de auditoria devem ser consultadas.                                                          |
| /subcategory |                                                                                                                  Uma ou mais subcategorias de auditoria especificadas por GUID ou nome.                                                                                                                  |
|     /SD      |                                                                                                        Recupera o descritor de segurança usado para delegar acesso à política de auditoria.                                                                                                        |
|   /Option    |                                                                              Recupera a política existente para as opções CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects ou AuditBasedirectories.                                                                               |
|      /r      |                                                                                                              Exibe a saída no formato de relatório, CSV (valor separado por vírgulas).                                                                                                              |
|      /?      |                                                                                                                             Exibe a ajuda no prompt de comando.                                                                                                                             |

## <a name="remarks"></a>Comentários
Todas as categorias e subcategorias podem ser especificadas pelo GUID ou pelo nome entre aspas. Os usuários podem ser especificados por SID ou nome.
para todas as operações get para a política por usuário e a política do sistema, você deve ter permissão de leitura nesse objeto definido no descritor de segurança. Você também pode executar operações Get por meio do direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar a operação get.
## <a name="BKMK_examples"></a>Disso
### <a name="examples-for-the-per-user-audit-policy"></a>Exemplos para a política de auditoria por usuário
Para recuperar a política de auditoria por usuário para a conta de convidado e exibir a saída para o sistema, controle detalhado e categorias de acesso a objeto, digite:
```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:"System","detailed Tracking","Object Access"
```
> [!NOTE]
> Esse comando é útil em dois cenários. Ao monitorar uma conta de usuário específica para atividades suspeitas, você pode usar o comando/Get para recuperar os resultados em categorias específicas usando uma política de inclusão para habilitar a auditoria adicional. Ou, se as configurações de auditoria em uma conta estiverem registrando vários eventos, mas supérfluos, você poderá usar o comando/Get para filtrar eventos estranhos para essa conta com uma política de exclusão. Para obter uma lista de todas as categorias, use o comando Auditpol/list/Category.
> Para recuperar a política de auditoria por usuário para uma categoria e uma determinada subcategoria, que relata as configurações inclusivas e exclusivas para essa subcategoria na categoria sistema da conta de convidado, digite:
> ```
> auditpol /get /user:guest /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> Para exibir a saída no formato de relatório e incluir o nome do computador, o destino da política, a subcategoria, o GUID da subcategoria, as configurações de inclusão e as configurações de exclusão, digite:
> ```
> auditpol /get /user:guest /category:detailed Tracking" /r
> ```
> ### <a name="examples-for-the-system-audit-policy"></a>Exemplos para a política de auditoria do sistema
> Para recuperar a política para a categoria e subcategorias do sistema, que relata as configurações de política de categoria e subcategoria para a política de auditoria do sistema, digite:
> ```
> auditpol /get /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> Para recuperar a política para a categoria de acompanhamento detalhado e subcategorias no formato de relatório e incluir o nome do computador, o destino da política, a subcategoria, o GUID da subcategoria, as configurações de inclusão e as configurações de exclusão, digite:
> ```
> auditpol /get /category:"detailed Tracking" /r
> ```
> Para recuperar a política para duas categorias com as categorias especificadas como GUIDs, que relata todas as configurações de política de auditoria de todas as subcategorias em duas categorias, digite:
> ```
> auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> ### <a name="examples-for-auditing-options"></a>Exemplos de opções de auditoria
> Para recuperar o estado, seja habilitado ou desabilitado, da opção AuditBaseObjects, digite:
> ```
> auditpol /get /option:AuditBaseObjects
> ```
> [!NOTE]
> As opções disponíveis são AuditBaseObjects, AuditBaseOperations e FullprivilegeAuditing.
> Para recuperar o estado habilitado, desabilitado ou 2 da opção CrashOnAuditFail, digite:
> ```
> auditpol /get /option:CrashOnAuditFail /r
> ```
> #### <a name="additional-references"></a>Referências adicionais
> [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
