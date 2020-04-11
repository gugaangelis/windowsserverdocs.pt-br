---
title: Bitsadmin util e enableanalyticchannel
description: Tópico de comandos do Windows para **Bitsadmin util e enableanalyticchannel**, que habilita ou desabilita o canal analítico do cliente bits.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8ff1f835415979036fdc0f8aa637fe693e57d46
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122681"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>Bitsadmin util e enableanalyticchannel

Habilita ou desabilita o canal analítico do cliente BITS.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| Parâmetro | Descrição |
| --------- | ---------- |
| VERDADEIRO ou falso | **True** ativa a validação de conteúdo para o arquivo especificado, enquanto **false** o desativa. |

## <a name="examples"></a>Exemplos

O exemplo a seguir habilita o canal analítico do cliente BITS.

```
C:\>bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)