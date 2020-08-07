---
title: ksetup domain
description: Artigo de referência para o comando de domínio ksetup, que define o nome de domínio para todas as operações Kerberos.
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81135ed668da901c55e891cec4c8749687359818
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887936"
---
# <a name="ksetup-domain"></a>ksetup domain

Define o nome de domínio para todas as operações Kerberos.

## <a name="syntax"></a>Sintaxe

```
ksetup /domain <domainname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<domainname>` | Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou uma forma simples do nome, como contoso.com ou contoso.|

### <a name="examples"></a>Exemplos

Para estabelecer uma conexão com um domínio válido, como a Microsoft, usando o `ksetup /mapuser` subcomando, digite:

```
ksetup /mapuser principal@realm domain-user /domain domain-name
```

Após uma conexão bem-sucedida, você receberá uma nova TGT ou um TGT existente será atualizado.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup mapuser](ksetup-mapuser.md)