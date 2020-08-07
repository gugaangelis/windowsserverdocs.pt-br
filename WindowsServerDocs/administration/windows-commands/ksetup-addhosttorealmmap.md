---
title: ksetup addhosttorealmmap
description: Artigo de referência para o comando ksetup addhosttorealmmap, que adiciona um mapeamento de SPN (nome da entidade de serviço) entre o host declarado e o realm.
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9722560a9ddebd01120dd60661ec895771ff9c6b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888147"
---
# <a name="ksetup-addhosttorealmmap"></a>ksetup addhosttorealmmap

Adiciona um mapeamento de SPN (nome da entidade de serviço) entre o host declarado e o realm. Esse comando também permite mapear um host ou vários hosts que estão compartilhando o mesmo sufixo DNS para o realm.

O mapeamento é armazenado no registro, em **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**.

## <a name="syntax"></a>Sintaxe

```
ksetup /addhosttorealmmap <hostname> <realmname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |------------ |
| `<hostname>` | O nome do host é o nome do computador e pode ser declarado como o nome de domínio totalmente qualificado do computador. |
| `<realmname>` | O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM. |

### <a name="examples"></a>Exemplos

Para mapear o computador host *IPops897* para o realm *contoso* , digite:

```
ksetup /addhosttorealmmap IPops897 CONTOSO
```

Verifique o registro para certificar-se de que o mapeamento ocorreu conforme o esperado.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup delhosttorealmmap](ksetup-delhosttorealmmap.md)
