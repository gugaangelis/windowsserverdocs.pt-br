---
title: nslookup set search
description: Artigo de referência para o comando Set Search do nslookup, que acrescenta os nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS à solicitação até que uma resposta seja recebida.
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91592daf702741fafcb88de792836f5183868101
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885546"
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
| pesquisar | Acrescenta os nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS para a solicitação até que uma resposta seja recebida. Este é o valor padrão. |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
