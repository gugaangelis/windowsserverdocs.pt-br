---
title: ksetup addkpasswd
description: Artigo de referência para o comando ksetup addkpasswd, que adiciona um endereço de servidor de senha Kerberos (kpasswd) para um realm.
ms.topic: reference
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62123fc8a04006078c42894ee53f11346dec0e59
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037944"
---
# <a name="ksetup-addkpasswd"></a>ksetup addkpasswd

Adiciona um endereço de servidor de senha Kerberos (kpasswd) para um realm.

## <a name="syntax"></a>Sintaxe

```
ksetup /addkpasswd <realmname> [<kpasswdname>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<realmname>` | Especifica o nome DNS em maiúsculas, como CORP. CONTOSO.COM e é listado como o realm ou **Realm padrão =** quando **ksetup** é executado. |
| `<kpasswdname>` | Especifica o servidor de senha Kerberos. Ele é declarado como um nome de domínio totalmente qualificado, que não diferencia maiúsculas de minúsculas, como mitkdc.contoso.com. Se o nome do KDC for omitido, o DNS poderá ser usado para localizar KDCs. |

#### <a name="remarks"></a>Comentários

- Se o realm Kerberos que a estação de trabalho estará Autenticando para dar suporte ao protocolo de alteração de senha Kerberos, você poderá configurar um computador cliente executando o sistema operacional Windows para usar um servidor de senha Kerberos.

- Você pode adicionar mais nomes KDC um de cada vez.

### <a name="examples"></a>Exemplos

Para configurar a CORP. CONTOSO.COM realm para usar o servidor KDC não Windows, mitkdc.contoso.com, como o servidor de senha, digite:

```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```

Para verificar se o nome do KDC está definido, digite `ksetup` e, em seguida, exiba a saída, procurando o texto, **kpasswd =**. Se você não vir o texto, significa que o mapeamento não foi configurado.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup delkpasswd](ksetup-delkpasswd.md)
