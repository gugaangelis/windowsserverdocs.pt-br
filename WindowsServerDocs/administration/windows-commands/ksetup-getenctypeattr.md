---
title: 'ksetup: getenctypeattr'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60de138ac73140c69e9a863083e01a51c0e13ca3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841529"
---
# <a name="ksetupgetenctypeattr"></a>ksetup: getenctypeattr



Recupera o atributo de tipo de criptografia para o domínio. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /getenctypeattr <DomainName> 
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<nome_do_domínio >|Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou uma forma simples do nome, como corp.contoso.com ou contoso.|

## <a name="remarks"></a>Comentários

Para exibir o tipo de criptografia para o tíquete de concessão de tíquete (TGT) e a chave de sessão do Kerberos, execute o comando **klist** e exiba a saída.

Se o comando for bem-sucedido ou falhar, uma mensagem de status será exibida após a conclusão bem-sucedida ou com falha.

Para definir o domínio ao qual você deseja se conectar e usar, execute o comando **ksetup/domain \<domainname >** .

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

Verifique o atributo de tipo de criptografia para o domínio:
```
ksetup /getenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>Referências adicionais

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)