---
title: ksetup:domain
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f53e807891b434709b1a8faed7aae8e8d444f6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857447"
---
# <a name="ksetupdomain"></a>ksetup:domain



Define o nome de domínio para todas as operações de Kerberos. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<DomainName>|Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou um formulário simples do nome, como contoso.com ou contoso.|

## <a name="remarks"></a>Comentários

Nenhum.

## <a name="BKMK_Examples"></a>Exemplos

Estabelece uma conexão a um domínio válido, como o Microsoft usando o subcomando /mapuser:
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Quando a conexão for bem-sucedida, você receberá um novo TGT ou um TGT existente será atualizado.

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)