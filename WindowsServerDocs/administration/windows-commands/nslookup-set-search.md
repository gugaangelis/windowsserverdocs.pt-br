---
title: nslookup set search
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d95ebe30ce45430787bebbfe63766a571a436bbf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436594"
---
# <a name="nslookup-set-search"></a>nslookup set search



Acrescenta os nomes de domínio do sistema de nome de domínio (DNS) na lista de pesquisa de domínio DNS para a solicitação até que uma resposta seja recebida. Isso se aplica quando o conjunto e a pesquisa contêm pelo menos um ponto de solicitação, mas não terminam com um ponto à direita.

## <a name="syntax"></a>Sintaxe

```
set [no]search
```

## <a name="parameters"></a>Parâmetros

|  Parâmetro   |                                                                          Descrição                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nosearch** |                            Interrompe a acrescentar os nomes de domínio do sistema de nome de domínio (DNS) na lista de pesquisa de domínio DNS para a solicitação.                            |
|  **search**  | Acrescenta os nomes de domínio do sistema de nome de domínio (DNS) na lista de pesquisa de domínio DNS para a solicitação até que uma resposta seja recebida. A sintaxe padrão é **pesquisa**. |
|    {Ajuda     |                                                                              ?}                                                                               |

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)