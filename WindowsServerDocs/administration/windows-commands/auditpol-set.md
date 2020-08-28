---
title: auditpol set
description: Artigo de referência para o comando de conjunto Auditpol, que define a política de auditoria por usuário, a política de auditoria do sistema ou as opções de auditoria.
ms.topic: reference
ms.assetid: f4947486-87bd-48cb-ba81-7230c8e70895
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 674de256eba3ee4b55f2b889717b7c2ed2defa3d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028964"
---
# <a name="auditpol-set"></a>auditpol set

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define a política de auditoria por usuário, a política de auditoria do sistema ou as opções de auditoria.

Para executar operações de *definição* nas políticas *por usuário* e *sistema* , você deve ter a permissão **gravar** ou **controle total** para esse objeto definido no descritor de segurança. Você também pode executar operações *set* se tiver o direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite acesso adicional que não é necessário para executar as operações de *conjunto* geral.

## <a name="syntax"></a>Sintaxe

```
auditpol /set
[/user[:<username>|<{sid}>][/include][/exclude]]
[/category:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/subcategory:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/option:<option name> /value: <enable>|<disable>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| / | A entidade de segurança para a qual a política de auditoria por usuário especificada pela categoria ou subcategoria está definida. A opção categoria ou subcategoria deve ser especificada, como um SID (identificador de segurança) ou um nome. |
| /include | Especificado com/user; indica que a política por usuário do usuário fará com que uma auditoria seja gerada mesmo que não seja especificada pela política de auditoria do sistema. Essa configuração é o padrão e será aplicada automaticamente se nem os parâmetros/include nem/Exclude forem especificados explicitamente. |
| /Exclude | Especificado com/user; indica que a política por usuário do usuário fará com que uma auditoria seja suprimida, independentemente da política de auditoria do sistema. Essa configuração é ignorada para os usuários que são membros do grupo Administradores local. |
| /category | Uma ou mais categorias de auditoria especificadas pelo GUID (identificador global exclusivo) ou pelo nome. Se nenhum usuário for especificado, a política do sistema será definida. |
| /subcategory | Uma ou mais subcategorias de auditoria especificadas por GUID ou nome. Se nenhum usuário for especificado, a política do sistema será definida. |
| /success | Especifica a auditoria com êxito. Essa configuração é o padrão e é aplicada automaticamente se nem os parâmetros/Success nem/Failure são explicitamente especificados. Essa configuração deve ser usada com um parâmetro que indica se a configuração será habilitada ou desabilitada. |
| /failure | Especifica a auditoria de falha. Essa configuração deve ser usada com um parâmetro que indica se a configuração será habilitada ou desabilitada. |
| /Option | Define a política de auditoria para as opções CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects ou AuditBasedirectories. |
| /SD | Define o descritor de segurança usado para delegar acesso à política de auditoria. O descritor de segurança deve ser especificado usando o SDDL (Security Descriptor Definition Language). O descritor de segurança deve ter uma DACL (lista de controle de acesso discricionário). |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para definir a política de auditoria por usuário para todas as subcategorias na categoria controle detalhado para o usuário Mikedan para que todas as tentativas bem-sucedidas do usuário sejam auditadas, digite:

```
auditpol /set /user:mikedan /category:detailed Tracking /include /success:enable
```

Para definir a política de auditoria por usuário para categorias especificadas por nome e GUID, e subcategorias especificadas pelo GUID para suprimir a auditoria para quaisquer tentativas bem-sucedidas ou com falha, digite:

```
auditpol /set /user:mikedan /exclude /category:Object Access,System,{6997984b-797a-11d9-bed3-505054503030}
/subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},:{0ccee9211-69ae-11d9-bed3-505054503030}, /success:enable /failure:enable
```

Para definir a política de auditoria por usuário para o usuário especificado para todas as categorias para a supressão da auditoria de todas as tentativas bem-sucedidas, digite:
```
auditpol /set /user:mikedan /exclude /category:* /success:enable
```

Para definir a política de auditoria do sistema para todas as subcategorias na categoria de acompanhamento detalhado para incluir a auditoria apenas para tentativas bem-sucedidas, digite:

```
auditpol /set /category:detailed Tracking /success:enable
```

> [!NOTE]
> A configuração de falha não é alterada.

Para definir a política de auditoria do sistema para as categorias de acesso a objeto e sistema (que é implícita porque as subcategorias estão listadas) e as subcategorias especificadas por GUIDs para a supressão de tentativas com falha e a auditoria de tentativas bem-sucedidas, digite:

```
auditpol /set /subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},{0ccee9211-69ae-11d9-bed3-505054503030}, /failure:disable /success:enable
```

Para definir as opções de auditoria para o estado habilitado para a opção CrashOnAuditFail, digite:

```
auditpol /set /option:CrashOnAuditFail /value:enable
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comandos Auditpol](auditpol.md)
