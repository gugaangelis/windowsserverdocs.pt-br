---
title: ksetup getenctypeattr
description: Artigo de referência para o comando ksetup getenctypeattr, que recupera o atributo de tipo de criptografia para o domínio.
ms.topic: reference
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f8dc2a1c7c4a2d17ca1fb0e5099fdb25a818d2aa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640031"
---
# <a name="ksetup-getenctypeattr"></a>ksetup getenctypeattr

Recupera o atributo de tipo de criptografia para o domínio. Uma mensagem de status é exibida após A conclusão bem-sucedida ou falha.

Você pode exibir o tipo de criptografia para o tíquete de concessão de tíquete (TGT) e a chave de sessão do Kerberos, executando o comando **klist** e exibindo a saída. Você pode definir o domínio para se conectar e usar o, executando o `ksetup /domain <domainname>` comando.

## <a name="syntax"></a>Sintaxe

```
ksetup /getenctypeattr <domainname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<domainname>` | Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou uma forma simples do nome, como corp.contoso.com ou contoso. |

### <a name="examples"></a>Exemplos

Para verificar o atributo de tipo de criptografia para o domínio, digite:

```
ksetup /getenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando klist](klist.md)

- [comando ksetup](ksetup.md)

- [comando de domínio ksetup](ksetup-domain.md)

- [comando ksetup addenctypeattr](ksetup-addenctypeattr.md)

- [comando ksetup setenctypeattr](ksetup-setenctypeattr.md)

- [comando ksetup delenctypeattr](ksetup-delenctypeattr.md)
