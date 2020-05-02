---
title: 'ksetup: getenctypeattr'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8363113d4fbb310d98b40d852b36a00f20320e6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724631"
---
# <a name="ksetupgetenctypeattr"></a>ksetup: getenctypeattr



Recupera o atributo de tipo de criptografia para o domínio.

## <a name="syntax"></a>Sintaxe

```
ksetup /getenctypeattr <DomainName> 
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> DomainName|Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou uma forma simples do nome, como corp.contoso.com ou contoso.|

## <a name="remarks"></a>Comentários

Para exibir o tipo de criptografia para o tíquete de concessão de tíquete (TGT) e a chave de sessão do Kerberos, execute o comando **klist** e exiba a saída.

Se o comando for bem-sucedido ou falhar, uma mensagem de status será exibida após a conclusão bem-sucedida ou com falha.

Para definir o domínio ao qual você deseja se conectar e usar, execute o comando **ksetup \</Domain nomedodomínio>** .

## <a name="examples"></a>Exemplos

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