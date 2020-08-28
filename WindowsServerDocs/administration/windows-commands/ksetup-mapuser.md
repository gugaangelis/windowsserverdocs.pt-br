---
title: ksetup mapuser
description: Artigo de referência para o comando ksetup mapuser, que mapeia o nome de uma entidade de segurança Kerberos para uma conta.
ms.topic: reference
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de3eeafdcd1ef94bf1e6c50009742b2981d0d7dc
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025460"
---
# <a name="ksetup-mapuser"></a>ksetup mapuser

Mapeia o nome de uma entidade de segurança Kerberos para uma conta.

## <a name="syntax"></a>Sintaxe

```
ksetup /mapuser <principal> <account>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<principal>` | Especifica o nome de domínio totalmente qualificado de qualquer usuário principal. Por exemplo, mike@corp.CONTOSO.COM. Se você não especificar um parâmetro de conta, o mapeamento será excluído para a entidade de segurança especificada. |
| `<account>` | Especifica qualquer nome de conta ou grupo de segurança que exista neste computador, como **convidado**, **usuários de domínio**ou **administrador**. Se esse parâmetro for omitido, o mapeamento será excluído para a entidade de segurança especificada. |

#### <a name="remarks"></a>Comentários

- Uma conta pode ser identificada especificamente, como **convidados do domínio**, ou você pode usar um caractere curinga (*) para incluir todas as contas.

- O computador só autenticará as entidades do Realm se apresentarem tíquetes Kerberos válidos.

- Sempre que forem feitas alterações no centro de distribuição de chaves externo (KDC) e na configuração de realm, será necessária uma reinicialização do computador em que a configuração foi alterada.

### <a name="examples"></a>Exemplos

Para ver as configurações mapeadas atuais e o realm padrão, digite:

```
ksetup
```

Para mapear a conta de Mike Danseglio no território Kerberos CONTOSO para a conta de convidado neste computador, concedendo a ele todos os privilégios de um membro da conta de convidado interna sem a necessidade de se autenticar neste computador, digite:

```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```

Para remover o mapeamento da conta de Mike Danseglio para a conta de convidado neste computador para impedir que ele se autentique neste computador com suas credenciais da CONTOSO, digite:

```
ksetup /mapuser mike@corp.CONTOSO.COM
```

Para mapear a conta de Mike Danseglio dentro do realm Kerberos CONTOSO para qualquer conta existente neste computador, digite:

```
ksetup /mapuser mike@corp.CONTOSO.COM *
```

> [!NOTE]
> Se apenas o usuário padrão e as contas de convidado estiverem ativas neste computador, os privilégios de Mike serão definidos para eles.

Para mapear todas as contas no realm Kerberos CONTOSO para qualquer conta existente de mesmo nome neste computador, digite:

```
ksetup /mapuser * *
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)
