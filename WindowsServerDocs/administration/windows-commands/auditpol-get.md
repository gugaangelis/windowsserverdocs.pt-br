---
title: auditpol get
description: Artigo de referência para o comando de obtenção do Auditpol, que recupera a política do sistema, a política por usuário, as opções de auditoria e o objeto do descritor de segurança de auditoria.
ms.topic: reference
ms.assetid: fe13de4e-836c-4207-b47c-64b6272d6c41
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 309e54170b7a154629a17e5fde1ed4943d0b180c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633225"
---
# <a name="auditpol-get"></a>auditpol get

> Aplica-se a: Windows Server (canal semestral), Windows Server, 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera a política do sistema, a política por usuário, as opções de auditoria e o objeto do descritor de segurança de auditoria.

Para executar operações *Get* nas políticas *por usuário* e *sistema* , você deve ter permissão de **leitura** para esse objeto definido no descritor de segurança. Você também pode executar operações *Get* se tiver o direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar as operações de *obtenção* geral.

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

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| / | Exibe a entidade de segurança para a qual a política de auditoria por usuário é consultada. O parâmetro/Category ou/Subcategory deve ser especificado. O usuário pode ser especificado como um SID (identificador de segurança) ou nome. Se nenhuma conta de usuário for especificada, a política de auditoria do sistema será consultada. |
| /category | Uma ou mais categorias de auditoria especificadas pelo GUID (identificador global exclusivo) ou pelo nome. Um asterisco (*) pode ser usado para indicar que todas as categorias de auditoria devem ser consultadas. |
| /subcategory | Uma ou mais subcategorias de auditoria especificadas por GUID ou nome. |
| /SD | Recupera o descritor de segurança usado para delegar acesso à política de auditoria. |
| /Option | Recupera a política existente para as opções CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects ou AuditBasedirectories. |
| /r | Exibe a saída no formato de relatório, CSV (valor separado por vírgulas). |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="remarks"></a>Comentários

Todas as categorias e subcategorias podem ser especificadas pelo GUID ou pelo nome entre aspas ("). Os usuários podem ser especificados por SID ou nome.

## <a name="examples"></a>Exemplos

Para recuperar a política de auditoria por usuário para a conta de convidado e exibir a saída para o sistema, controle detalhado e categorias de acesso a objeto, digite:

```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:System,detailed Tracking,Object Access
```

> [!NOTE]
> Esse comando é útil em dois cenários. 1) ao monitorar uma conta de usuário específica para atividades suspeitas, você pode usar o `/get` comando para recuperar os resultados em categorias específicas usando uma política de inclusão para habilitar a auditoria adicional. 2) se as configurações de auditoria em uma conta estiverem registrando vários eventos, mas supérfluos, você poderá usar o `/get` comando para filtrar eventos estranhos para essa conta com uma política de exclusão. Para obter uma lista de todas as categorias, use o `auditpol /list /category` comando.

Para recuperar a política de auditoria por usuário para uma categoria e uma determinada subcategoria, que relata as configurações inclusivas e exclusivas para essa subcategoria na categoria sistema da conta de convidado, digite:

```
auditpol /get /user:guest /category:System /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

Para exibir a saída no formato de relatório e incluir o nome do computador, o destino da política, a subcategoria, o GUID da subcategoria, as configurações de inclusão e as configurações de exclusão, digite:

```
auditpol /get /user:guest /category:detailed Tracking /r
```

Para recuperar a política para a categoria e subcategorias do sistema, que relata as configurações de política de categoria e subcategoria para a política de auditoria do sistema, digite:

```
auditpol /get /category:System /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

Para recuperar a política para a categoria de acompanhamento detalhado e subcategorias no formato de relatório e incluir o nome do computador, o destino da política, a subcategoria, o GUID da subcategoria, as configurações de inclusão e as configurações de exclusão, digite:

```
auditpol /get /category:detailed Tracking /r
```

Para recuperar a política para duas categorias com as categorias especificadas como GUIDs, que relata todas as configurações de política de auditoria de todas as subcategorias em duas categorias, digite:

```
auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

Para recuperar o estado, seja habilitado ou desabilitado, da opção AuditBaseObjects, digite:

```
auditpol /get /option:AuditBaseObjects
```

Onde as opções disponíveis são AuditBaseObjects, AuditBaseOperations e FullprivilegeAuditing. Para recuperar o estado habilitado, desabilitado ou 2 da opção CrashOnAuditFail, digite:

```
auditpol /get /option:CrashOnAuditFail /r
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comandos Auditpol](auditpol.md)
