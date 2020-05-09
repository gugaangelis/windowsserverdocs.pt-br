---
title: diskperf
description: Tópico de referência para o comando diskperf, que pode ser usado para habilitar ou desabilitar remotamente os contadores de desempenho de disco físico ou lógico em computadores que executam o Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 092518f414d6e27436c46ffd6f9f15b6e6c0407e
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992433"
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
