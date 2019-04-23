---
title: ksetup:delenctypeattr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c2cc96e8156cafd3846422596abe62513e275b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838127"
---
# <a name="ksetupdelenctypeattr"></a>ksetup:delenctypeattr



Remove o atributo de tipo de criptografia para o domínio. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /delenctypeattr <DomainName> 
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<DomainName>|Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou um formulário simples do nome, como corp.contoso.com ou contoso.|

## <a name="remarks"></a>Comentários

Para exibir o tipo de criptografia para o tíquete de concessão de tíquete do Kerberos (TGT) e a chave de sessão, execute as **klist** de comando e exibir a saída.

Uma mensagem de status é exibida após a conclusão com êxito ou falha.

Para definir o domínio que você deseja conectar e usar, execute as **ksetup /domain \<DomainName >** comando.

## <a name="BKMK_Examples"></a>Exemplos

Determine os tipos de criptografia atuais que são definidos neste computador:
```
klist
```
Defina o domínio para mit.contoso.com:
```
ksetup /domain mit.contoso.com
```
Verifique se o que é o atributo de tipo de criptografia para o domínio:
```
ksetup /getenctypeattr mit.contoso.com
```
Remova o atributo de tipo do conjunto de criptografia para o domínio mit.contoso.com:
```
ksetup /delenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>Referências adicionais

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)