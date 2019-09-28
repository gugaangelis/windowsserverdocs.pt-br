---
title: 'ksetup: mapuser'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b80538999c364e9ed10ca0ed43387f603ac9ad3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374983"
---
# <a name="ksetupmapuser"></a>ksetup: mapuser



Mapeia o nome de uma entidade de segurança Kerberos para uma conta. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /mapuser <Principal> <Account>
```

### <a name="parameters"></a>Parâmetros

|  Parâmetro   |                                                   Descrição                                                   |
|--------------|-----------------------------------------------------------------------------------------------------------------|
| \<Principal > |              O nome de domínio totalmente qualificado de qualquer entidade; por exemplo, mike@corp.CONTOSO.COM.              |
|  \<Account >  | Qualquer nome de conta ou grupo de segurança existente neste computador, como convidado, usuários de domínio ou administrador. |

## <a name="remarks"></a>Comentários

Uma conta pode ser identificada especificamente, como convidados do domínio. Ou você pode usar o caractere curinga (*) para incluir todas as contas.

Se um nome de conta for omitido, o mapeamento será excluído para a entidade de segurança especificada.

O computador só autenticará as entidades do Realm se apresentarem tíquetes Kerberos válidos.

Use **ksetup** sem parâmetros ou argumentos para ver as configurações mapeadas atuais e o realm padrão.

Sempre que forem feitas alterações no centro de distribuição de chaves externo (KDC) e na configuração de realm, será necessária uma reinicialização do computador em que a configuração foi alterada.

## <a name="BKMK_Examples"></a>Disso

Mapeie a conta de Mike Danseglio no território Kerberos CONTOSO para a conta de convidado neste computador, concedendo a ele todos os privilégios de um membro da conta convidado interna sem a necessidade de se autenticar neste computador:
```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```
Remova o mapeamento da conta de Mike Danseglio para a conta de convidado neste computador para impedir que ele se autentique neste computador com suas credenciais da CONTOSO:
```
ksetup /mapuser mike@corp.CONTOSO.COM 
```
Mapeie a conta de Mike Danseglio no realm Kerberos CONTOSO para qualquer conta existente neste computador. (se apenas o usuário padrão e as contas de convidado estiverem ativas neste computador, os privilégios de Mike serão definidos para eles):
```
ksetup /mapuser mike@corp.CONTOSO.COM *
```
Mapeie todas as contas no realm Kerberos CONTOSO para qualquer conta existente de mesmo nome neste computador:
```
ksetup /mapuser * *
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)