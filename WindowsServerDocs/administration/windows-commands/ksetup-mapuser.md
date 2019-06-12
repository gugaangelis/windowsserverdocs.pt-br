---
title: ksetup:mapuser
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5bc68fe9e8f4cbb9869cb74e4eb20a3400eb56ad
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437967"
---
# <a name="ksetupmapuser"></a>ksetup:mapuser



Mapeia o nome de uma entidade de segurança do Kerberos em uma conta. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /mapuser <Principal> <Account>
```

### <a name="parameters"></a>Parâmetros

|  Parâmetro   |                                                   Descrição                                                   |
|--------------|-----------------------------------------------------------------------------------------------------------------|
| \<Entidade de segurança > |              O nome de domínio totalmente qualificado de qualquer entidade; Por exemplo, mike@corp.CONTOSO.COM.              |
|  \<Conta >  | Qualquer nome de grupo de segurança ou a conta que existe nesse computador, como convidado, os usuários do domínio ou administrador. |

## <a name="remarks"></a>Comentários

Uma conta pode ser especificamente identificada, como convidados do domínio. Ou você pode usar o caractere curinga (*) para incluir todas as contas.

Se um nome de conta for omitido, o mapeamento é excluído para a entidade especificada.

O computador só irá autenticar as entidades de segurança do realm determinado se apresentam os tíquetes Kerberos válidos.

Use **ksetup** sem parâmetros ou argumentos, para ver a atual mapeado configurações e o domínio padrão.

Sempre que forem feitas alterações para o Centro de distribuição de chaves (KDC) externa e a configuração de território, uma reinicialização do computador em que a configuração foi alterada é necessária.

## <a name="BKMK_Examples"></a>Exemplos

Mapear a conta de Mike Danseglio dentro do realm do Kerberos CONTOSO para a conta de convidado neste computador, concedendo a ele todos os privilégios de um membro da conta de convidado interna sem precisar se autenticar neste computador:
```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```
Remova o mapeamento de conta de Mike Danseglio para a conta de convidado neste computador para evitar que ele autenticar este computador com suas credenciais da CONTOSO:
```
ksetup /mapuser mike@corp.CONTOSO.COM 
```
Mapear a conta de Mike Danseglio dentro do realm do Kerberos da CONTOSO para qualquer conta existente neste computador. (se apenas o usuário padrão e contas de convidado estiverem ativas neste computador, os privilégios de Mike serão definidos como aqueles):
```
ksetup /mapuser mike@corp.CONTOSO.COM *
```
Mapear todas as contas no realm do Kerberos da CONTOSO para qualquer conta existente de mesmo nome neste computador:
```
ksetup /mapuser * *
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)