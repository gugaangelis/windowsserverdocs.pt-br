---
title: bitsadmin util e enableanalyticchannel
description: Artigo de referência para o comando Bitsadmin util e enableanalyticchannel, que habilita ou desabilita o canal analítico do cliente BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 515402f42dee54baa662f37718841f70b1cd2882
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927415"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>bitsadmin util e enableanalyticchannel

Habilita ou desabilita o canal analítico do cliente BITS.

## <a name="syntax"></a>Syntax

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
