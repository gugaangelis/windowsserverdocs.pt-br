---
title: ksetup changepassword
description: Artigo de referência para o comando ChangePassword ksetup, que usa o valor de centro de distribuição de chaves (KDC) senha (kpasswd) para alterar a senha do usuário conectado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 283078e7-a88f-4875-90e6-f8605e6b7ea7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e49a9c0a796357c89efd3c86373c77468670176c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925500"
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
