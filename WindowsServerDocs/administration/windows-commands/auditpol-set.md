---
title: auditpol set
description: Tópico de comandos do Windows para **auditpol set** – define a política de auditoria por usuário, a política de auditoria do sistema ou opções de auditoria.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4947486-87bd-48cb-ba81-7230c8e70895
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8778401efb272a167aaa3d9abb4ecafc67e5f50d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435117"
---
# <a name="auditpol-set"></a>auditpol set

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Política de auditoria de conjuntos, cada usuário, política de auditoria do sistema ou opções de auditoria.

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
## <a name="parameters"></a>Parâmetros

|  Parâmetro   |                                                                                                                                          Descrição                                                                                                                                           |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /user     |                                        A entidade de segurança para os quais cada usuário especificada por categoria ou subcategoria de política de auditoria é definida. Opção de categoria ou subcategoria deve ser especificada como um identificador de segurança (SID) ou o nome.                                         |
|   /include   | Especificado com /user; indica que a política do usuário por usuário fará com que uma auditoria a ser gerado mesmo se não for especificado, a política de auditoria do sistema. Essa configuração é o padrão e é aplicada automaticamente se nem o / inclui nem /exclude parâmetros são especificados explicitamente. |
|   /exclude   |                                Especificado com /user; indica que a política do usuário por usuário fará com que uma auditoria a serem suprimidos, independentemente da política de auditoria do sistema. Essa configuração é ignorada para os usuários que são membros do grupo Administradores local.                                |
|  /category   |                                                                            Uma ou mais categorias de auditoria especificadas por nome ou identificador global exclusivo (GUID). Se nenhum usuário for especificado, a diretiva do sistema é definida.                                                                             |
| /subcategory |                                                                                         Um ou mais subcategorias de auditoria especificadas por nome ou GUID. Se nenhum usuário for especificado, a diretiva do sistema é definida.                                                                                          |
|   /success   |                 Especifica a auditoria de êxito. Essa configuração é o padrão e é aplicada automaticamente se nem o /success nem /failure são explicitamente especificados. Essa configuração deve ser usada com um parâmetro que indica se deseja habilitar ou desabilitar a configuração.                 |
|   /failure   |                                                                                  Especifica a auditoria de falha. Essa configuração deve ser usada com um parâmetro que indica se deseja habilitar ou desabilitar a configuração.                                                                                   |
|   /option    |                                                                                   Define a política de auditoria para as opções CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects ou AuditBasedirectories.                                                                                    |
|     /sd      |                 Define o descritor de segurança usado para delegar acesso para a política de auditoria. O descritor de segurança deve ser especificado usando a definição de linguagem SDDL (Security Descriptor). O descritor de segurança deve ter uma lista de controle de acesso discricionário (DACL).                 |
|      /?      |                                                                                                                              Exibe a ajuda no prompt de comando.                                                                                                                              |

## <a name="remarks"></a>Comentários
todas as operações de conjunto para a política por usuário e a diretiva do sistema, você deve escrever ou conjunto de permissões de controle total nesse objeto no descritor de segurança. Você também pode executar operações de conjunto que possui o **gerenciar o log de auditoria e segurança** direito de usuário (SeSecurityPrivilege). No entanto, esse direito permite acesso adicional que não é necessário para executar a operação de definição.
## <a name="BKMK_examples"></a>Exemplos
### <a name="examples-for-the-per-user-audit-policy"></a>Exemplos para a política de auditoria por usuário
Para definir cada usuário a política de auditoria de todas as subcategorias sob a categoria de rastreamento detalhada para o usuário mikedan para que todas as tentativas bem-sucedidas do usuário serão auditadas, digite:
```
auditpol /set /user:mikedan /category:"detailed Tracking" /include /success:enable
```
Para definir a política de auditoria por usuário para especificado pelo nome e GUID especificadas pelo GUID para suprimir a auditoria para quaisquer tentativas com êxito ou falhas de subcategorias de categorias, digite:
```
auditpol /set /user:mikedan /exclude /category:"Object Access","System",{6997984b-797a-11d9-bed3-505054503030} 
/subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},:{0ccee9211-69ae-11d9-bed3-505054503030}, /success:enable /failure:enable
```
Para definir a política de auditoria por usuário para o usuário especificado para todas as categorias para a supressão de auditoria de todas as tentativas bem-sucedidas, mas, digite:
```
auditpol /set /user:mikedan /exclude /category:* /success:enable
```
### <a name="examples-for-the-system-audit-policy"></a>Exemplos para a política de auditoria do sistema
Para definir a política de auditoria do sistema para todas as subcategorias sob a categoria de rastreamento detalhada para incluir a auditoria para tentativas apenas com êxito, digite:
```
auditpol /set /category:"detailed Tracking" /success:enable
```
> [!NOTE]
> A configuração de falha não é alterada.
> Para definir a política de auditoria do sistema para as categorias de acesso a objetos e do sistema (que é implícito porque as subcategorias são listadas) e as subcategorias especificadas por GUIDs para a supressão de tentativas com falha e a auditoria de tentativas com êxito, digite:
> ```
> auditpol /set /subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},{0ccee9211-69ae-11d9-bed3-505054503030}, /failure:disable /success:enable
> ```
> ### <a name="example-for-auditing-options"></a>Exemplo de opções de auditoria
> Para definir opções de auditoria para o estado habilitado para a opção CrashOnAuditFail, digite:
> ```
> auditpol /set /option:CrashOnAuditFail /value:enable
> ```
> #### <a name="additional-references"></a>Referências adicionais
> [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
