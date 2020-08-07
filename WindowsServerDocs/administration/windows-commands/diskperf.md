---
title: diskperf
description: Artigo de referência para o comando diskperf, que pode ser usado para habilitar ou desabilitar remotamente os contadores de desempenho de disco físico ou lógico em computadores que executam o Windows.
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81cefe217abaa7b2d4ee843f3887076f66484422
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890861"
---
# <a name="diskperf"></a>diskperf

O comando **diskperf** habilita ou desabilita remotamente os contadores de desempenho de disco físico ou lógico em computadores que executam o Windows.

## <a name="syntax"></a>Sintaxe

```
diskperf [-y[d|v] | -n[d|v]] [\\computername]
```

## <a name="options"></a>Opções

| Opção | Descrição |
| ------ | ----------- |
| -y | Inicia todos os contadores de desempenho de disco quando o computador é reiniciado. |
| -yd | Habilita contadores de desempenho de disco para unidades físicas quando o computador é reiniciado. |
| -yv | Habilita contadores de desempenho de disco para unidades lógicas ou volumes de armazenamento quando o computador é reiniciado. |
| -n | Desabilita todos os contadores de desempenho de disco quando o computador é reiniciado. |
| -nd | Desabilite os contadores de desempenho de disco para unidades físicas quando o computador for reiniciado. |
| -NV | Desabilite os contadores de desempenho de disco para unidades lógicas ou volumes de armazenamento quando o computador for reiniciado. |
| `\\<computername>` | Especifica o nome do computador no qual você deseja habilitar ou desabilitar os contadores de desempenho de disco. |
| -? | Exibe a ajuda contextual. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
