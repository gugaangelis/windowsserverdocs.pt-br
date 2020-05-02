---
title: bitsadmin util e enableanalyticchannel
description: Tópico de referência para o comando Bitsadmin util e enableanalyticchannel, que habilita ou desabilita o canal analítico do cliente BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1f5c8c924d1011928aca6ec1bcebd4d71abb015
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707768"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>bitsadmin util e enableanalyticchannel

Habilita ou desabilita o canal analítico do cliente BITS.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| Parâmetro | Descrição |
| --------- | ---------- |
| VERDADEIRO ou falso | **True** ativa a validação de conteúdo para o arquivo especificado, enquanto **false** o desativa. |

## <a name="examples"></a>Exemplos

Para ativar ou desativar o canal analítico do cliente do BITS.

```
bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin util](bitsadmin-util.md)

- [comando Bitsadmin](bitsadmin.md)
