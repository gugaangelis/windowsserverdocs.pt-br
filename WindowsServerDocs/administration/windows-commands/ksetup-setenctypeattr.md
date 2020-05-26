---
title: ksetup setenctypeattr
description: Tópico de referência para o comando ksetup setenctypeattr, que define o atributo de tipo de criptografia para o domínio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 88fb913e-6b57-48d9-8c16-a035ab2977ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e76ad3d08505208346ff2a3e100194239187953d
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817356"
---
# <a name="ksetup-setenctypeattr"></a>ksetup setenctypeattr

Define o atributo de tipo de criptografia para o domínio. Uma mensagem de status é exibida após A conclusão bem-sucedida ou falha.

Você pode exibir o tipo de criptografia para o tíquete de concessão de tíquete (TGT) e a chave de sessão do Kerberos, executando o comando **klist** e exibindo a saída. Você pode definir o domínio para se conectar e usar o, executando o `ksetup /domain <domainname>` comando.

## <a name="syntax"></a>Sintaxe

```
ksetup /setenctypeattr <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<domainname>` | Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou uma forma simples do nome, como corp.contoso.com ou contoso. |
| tipo de criptografia | Deve ser um dos seguintes tipos de criptografia com suporte:<ul><li>DES-CBC-CRC</li><li>DES-CBC-MD5</li><li>RC4-HMAC-MD5</li><li>AES128-CTS-HMAC-SHA1-96</li><li>AES256-CTS-HMAC-SHA1-96</li></ul> |

#### <a name="remarks"></a>Comentários

- Você pode definir ou adicionar vários tipos de criptografia separando os tipos de criptografia no comando com um espaço. No entanto, você só pode fazer isso para um domínio de cada vez.

### <a name="examples"></a>Exemplos

Para exibir o tipo de criptografia para o tíquete de concessão de tíquete (TGT) e a chave de sessão do Kerberos, digite:

```
klist
```

Para definir o domínio como corp.contoso.com, digite:

```
ksetup /domain corp.contoso.com
```

Para definir o atributo de tipo de criptografia como AES-256-CTS-HMAC-SHA1-96 para o domínio corp.contoso.com, digite:

```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```

Para verificar se o atributo de tipo de criptografia foi definido como pretendido para o domínio, digite:

```
ksetup /getenctypeattr corp.contoso.com
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando klist](klist.md)

- [comando ksetup](ksetup.md)

- [comando de domínio ksetup](ksetup-domain.md)

- [comando ksetup addenctypeattr](ksetup-addenctypeattr.md)

- [comando ksetup getenctypeattr](ksetup-getenctypeattr.md)

- [comando ksetup delenctypeattr](ksetup-delenctypeattr.md)
