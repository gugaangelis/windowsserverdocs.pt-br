---
title: 'ksetup: domínio'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f127eaf33e9ef6d597851c31a4167ceaa3516abb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724683"
---
# <a name="ksetupdomain"></a>ksetup: domínio



Define o nome de domínio para todas as operações Kerberos.

## <a name="syntax"></a>Sintaxe

```
ksetup /domain <DomainName>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> DomainName|Nome do domínio ao qual você deseja estabelecer uma conexão. Use o nome de domínio totalmente qualificado ou uma forma simples do nome, como contoso.com ou contoso.|

## <a name="remarks"></a>Comentários

Nenhum.

## <a name="examples"></a>Exemplos

Estabeleça uma conexão com um domínio válido, como a Microsoft usando o subcomando/mapuser:
```
ksetup /mapuser principal@realm domain-user /domain domain-name
```
Quando a conexão for bem-sucedida, você receberá uma nova TGT ou um TGT existente será atualizado.

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)