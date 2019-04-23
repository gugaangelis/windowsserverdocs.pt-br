---
title: ksetup:getenctypeattr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 302f94616f98eb350332b08ad37a58305a0a0be1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841987"
---
# <a name="ksetupgetenctypeattr"></a>ksetup:getenctypeattr



Recupera o atributo de tipo de criptografia para o domínio. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /getenctypeattr <DomainName> 
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<DomainName>|Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou um formulário simples do nome, como corp.contoso.com ou contoso.|

## <a name="remarks"></a>Comentários

Para exibir o tipo de criptografia para o tíquete de concessão de tíquete do Kerberos (TGT) e a chave de sessão, execute as **klist** de comando e exibir a saída.

Se o comando for bem-sucedido ou falha, uma mensagem de status é exibida após a conclusão com êxito ou falha.

Para definir o domínio que você deseja conectar e usar, execute as **ksetup /domain \<DomainName >** comando.

## <a name="BKMK_Examples"></a>Exemplos

Verifique se o atributo de tipo de criptografia para o domínio:
```
ksetup /getenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>Referências adicionais

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)