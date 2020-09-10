---
title: ksetup changepassword
description: Artigo de referência para o comando ChangePassword ksetup, que usa o valor de centro de distribuição de chaves (KDC) senha (kpasswd) para alterar a senha do usuário conectado.
ms.topic: reference
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ff88136c51b9790867c76d54467996e9305cbc93
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639714"
---
# <a name="ksetup-changepassword"></a>ksetup changepassword

Usa o valor de centro de distribuição de chaves (KDC) senha (kpasswd) para alterar a senha do usuário conectado. A saída do comando informa o status de êxito ou falha.

Você pode verificar se o **kpasswd** está definido, executando o `ksetup /dumpstate` comando e exibindo a saída.


## <a name="syntax"></a>Sintaxe

```
ksetup /changepassword <oldpassword> <newpassword>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<oldpassword>` | Especifica a senha existente do usuário conectado. |
| `<newpassword>` | Especifica a nova senha do usuário conectado. Essa senha deve atender a todos os requisitos de senha definidos neste computador. |

#### <a name="remarks"></a>Comentários

- Se a conta de usuário não for encontrada no domínio atual, o sistema solicitará que você forneça o nome de domínio onde a conta de usuário reside.

- Se você quiser forçar uma alteração de senha no próximo logon, esse comando permitirá o uso do asterisco (*), de modo que o usuário será solicitado a fornecer uma nova senha.

-

### <a name="examples"></a>Exemplos

Para alterar a senha de um usuário que está conectado atualmente neste computador neste domínio, digite:

```
ksetup /changepassword Pas$w0rd Pa$$w0rd
```

Para alterar a senha de um usuário que está conectado no momento no domínio contoso, digite:

```
ksetup /domain CONTOSO /changepassword Pas$w0rd Pa$$w0rd
```

Para forçar o usuário conectado no momento a alterar a senha no próximo logon, digite:

```
ksetup /changepassword Pas$w0rd *
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup dumpstate](ksetup-dumpstate.md)

- [comando ksetup addkpasswd](ksetup-addkpasswd.md)

- [comando ksetup delkpasswd](ksetup-delkpasswd.md)

- [comando ksetup dumpstate](ksetup-dumpstate.md)
