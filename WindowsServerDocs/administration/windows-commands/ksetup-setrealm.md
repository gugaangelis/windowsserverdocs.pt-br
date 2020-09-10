---
title: ksetup setrealm
description: Artigo de referência para o comando ksetup setrealm, que define o nome de um realm Kerberos.
ms.topic: reference
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a06c5fef1127236319b580fbd6810269c58d1377
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627135"
---
# <a name="ksetup-setrealm"></a>ksetup setrealm

Define o nome de um realm Kerberos.

> [!IMPORTANT]
> Não há suporte para a definição do realm Kerberos em um controlador de domínio. A tentativa de fazer isso causa um aviso e uma falha de comando.

## <a name="syntax"></a>Sintaxe

```
ksetup /setrealm <DNSdomainname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<DNSdomainname>` | Especifica o nome DNS em maiúsculas, como CORP. CONTOSO.COM. Você pode usar o nome de domínio totalmente qualificado ou uma forma simples do nome. Se você não usar letras maiúsculas para o nome DNS, será solicitado que você faça a verificação para continuar. |

### <a name="examples"></a>Exemplos

Para definir o realm deste computador para um nome de domínio específico e para restringir o acesso por um controlador que não seja de domínio apenas ao realm Kerberos do CONTOSO, digite:

```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [ksetup removerealm](ksetup-removerealm.md)
