---
title: bitsadmin nowrap
description: Artigo de referência para o comando nowrap Bitsadmin, que trunca qualquer linha de texto de saída que se estenda além da borda mais à direita da janela de comando.
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72c50bdf7d19ea4603fc232dcf54acdfd97d3f17
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893644"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Trunca qualquer linha de texto de saída que ultrapasse a borda mais à direita da janela de comando. Por padrão, todas as opções, exceto a opção de **Monitor** , encapsulam a saída. Especifique a opção **nowrap** antes de outras opções.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /nowrap
```

## <a name="examples"></a>Exemplos

Para recuperar o estado do trabalho chamado *myDownloadJob* enquanto não estiver encapsulando a saída:

```
bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
