---
title: 'ksetup: setenctypeattr'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 88fb913e-6b57-48d9-8c16-a035ab2977ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4cb7380a5fc65734902c6eed0b4b941eda6f6f5a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724568"
---
# <a name="ksetupsetenctypeattr"></a>ksetup: setenctypeattr



Define o atributo de tipo de criptografia para o domínio.

## <a name="syntax"></a>Sintaxe

```
ksetup /setenctypeattr <Domain name> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> DomainName|Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou uma forma simples do nome, como corp.contoso.com ou contoso.|
|Tipo de criptografia|Deve ser um dos seguintes tipos de criptografia com suporte:</br>-DES-CBC-CRC</br>-DES-CBC-MD5</br>-RC4-HMAC-MD5</br>-AES128-CTS-HMAC-SHA1-96</br>-AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>Comentários

Para exibir o tipo de criptografia para o tíquete de concessão de tíquete (TGT) e a chave de sessão do Kerberos, execute o comando **klist** e exiba a saída.

Você pode definir ou adicionar vários tipos de criptografia separando os tipos de criptografia no comando com um espaço. No entanto, você só pode fazer isso para um domínio de cada vez.

Se o comando tiver êxito ou falhar, será exibida uma mensagem de status.

Para definir o domínio ao qual você deseja se conectar e usar, execute o comando **ksetup \</Domain nomedodomínio>** .

## <a name="examples"></a>Exemplos

Determine os tipos de criptografia atuais que estão definidos neste computador:
```
klist
```
Defina o domínio como corp.contoso.com:
```
ksetup /domain corp.contoso.com
```
Defina o atributo de tipo de criptografia como AES-256-CTS-HMAC-SHA1-96 para o domínio corp.contoso.com:
```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
Verifique se o atributo de tipo de criptografia foi definido como pretendido para o domínio:
```
ksetup /getenctypeattr corp.contoso.com
```

## <a name="additional-references"></a>Referências adicionais

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)