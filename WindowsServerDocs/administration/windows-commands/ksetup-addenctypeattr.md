---
title: ksetup:addenctypeattr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 32cc87d7-b9e1-4d14-9eb7-3b439c55aa3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 711da6a81269fc838ca091765ddbcac63c3fe6e4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886387"
---
# <a name="ksetupaddenctypeattr"></a>ksetup:addenctypeattr



Adiciona o atributo de tipo de criptografia para a lista de tipos possíveis para o domínio. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /addenctypeattr <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<DomainName>|Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou um formulário simples do nome, como corp.contoso.com ou contoso.|
|Tipo de criptografia|Deve ser um dos seguintes tipos de criptografia com suporte:</br>-   DES-CBC-CRC</br>-   DES-CBC-MD5</br>-   RC4-HMAC-MD5</br>-   AES128-CTS-HMAC-SHA1-96</br>-   AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>Comentários

Para exibir o tipo de criptografia para o tíquete de concessão de tíquete do Kerberos (TGT) e a chave de sessão, execute as **klist** de comando e exibir a saída.

Você pode definir ou adicionar vários tipos de criptografia, separando os tipos de criptografia no comando com um espaço. No entanto, você pode fazer isso somente para um domínio por vez.

Se o comando for bem-sucedido ou falha, será exibida uma mensagem de status.

Para definir o domínio que você deseja conectar e usar, execute as **ksetup /domain \<DomainName >** comando.

## <a name="BKMK_Examples"></a>Exemplos

Determine os tipos de criptografia atuais que são definidos neste computador:
```
klist
```
Defina o domínio corp.contoso.com:
```
ksetup /domain corp.contoso.com
```
Adicione o tipo de criptografia AES-256-CTS-HMAC-SHA1-96 à lista de tipos possíveis para o domínio corp.contoso.com:
```
ksetup /addenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
Defina o atributo de tipo de criptografia como AES-256-CTS-HMAC-SHA1-96 para o domínio corp.contoso.com:
```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
Verifique se o atributo de tipo de criptografia foi definido conforme o esperado para o domínio:
```
ksetup /getenctypeattr corp.contoso.com
```

#### <a name="additional-references"></a>Referências adicionais

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)