---
title: nslookup set search
description: Artigo de referência para o comando Set Search do nslookup, que acrescenta os nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS à solicitação até que uma resposta seja recebida.
ms.topic: reference
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17cee7cbc2112db346c91ab4a7b88264eff6a850
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025150"
---
# <a name="nslookup-set-search"></a>nslookup set search

Acrescenta os nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS à solicitação até que uma resposta seja recebida. Isso se aplica quando o conjunto e a solicitação de pesquisa contêm pelo menos um ponto, mas não terminam com um ponto à direita.

## <a name="syntax"></a>Sintaxe

```
set [no]search
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| Nosearch | Interrompe a anexação dos nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS para a solicitação. |
| pequisa | Acrescenta os nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS para a solicitação até que uma resposta seja recebida. Esse é o valor padrão. |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
