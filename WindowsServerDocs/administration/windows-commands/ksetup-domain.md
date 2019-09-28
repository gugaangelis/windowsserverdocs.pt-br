---
title: 'ksetup: domínio'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0a4d9f09def32c7518046c25887f4154020c5d7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375121"
---
# <a name="ksetupdomain"></a>ksetup: domínio



Define o nome de domínio para todas as operações Kerberos. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /domain <DomainName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<DomainName >|Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou uma forma simples do nome, como contoso.com ou contoso.|

## <a name="remarks"></a>Comentários

nenhuma.

## <a name="BKMK_Examples"></a>Disso

Estabeleça uma conexão com um domínio válido, como a Microsoft usando o subcomando/mapuser:
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Quando a conexão for bem-sucedida, você receberá uma nova TGT ou um TGT existente será atualizado.

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)