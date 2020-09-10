---
title: ksetup delkpasswd
description: Artigo de referência para o comando ksetup delkpasswd, que remove um servidor de senha Kerberos (kpasswd) para um realm.
ms.topic: reference
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 52624252953770ebe131a7c31618058232a21d05
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639678"
---
# <a name="ksetup-delkpasswd"></a>ksetup delkpasswd

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um servidor de senha Kerberos (kpasswd) para um realm.

## <a name="syntax"></a>Sintaxe

```
ksetup /delkpasswd <realmname> <kpasswdname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<realmname>` |  Especifica o nome DNS em maiúsculas, como CORP. CONTOSO.COM e é listado como o realm ou **Realm padrão =** quando **ksetup** é executado. |
| `<kpasswdname>` | Especifica o servidor de senha Kerberos. Ele é declarado como um nome de domínio totalmente qualificado, que não diferencia maiúsculas de minúsculas, como mitkdc.contoso.com. Se o nome do KDC for omitido, o DNS poderá ser usado para localizar KDCs. |

### <a name="examples"></a>Exemplos

Para garantir o realm CORP. CONTOSO.COM usa o mitkdc.contoso.com do servidor KDC não Windows como o servidor de senha, digite:

```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```

Para garantir o realm CORP. CONTOSO.COM não está mapeado para um servidor de senha Kerberos (o nome do KDC), digite `ksetup` no computador com Windows e, em seguida, exiba a saída.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup delkpasswd](ksetup-delkpasswd.md)
