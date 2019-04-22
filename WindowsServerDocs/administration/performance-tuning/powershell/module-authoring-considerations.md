---
title: Considerações de criação de módulo do PowerShell
description: Considerações de criação de módulo do PowerShell
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 37dd860019b91daf70947dba93d20274048487a0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818717"
---
# <a name="powershell-module-authoring-considerations"></a>Considerações de criação de módulo do PowerShell

Este documento inclui algumas diretrizes relacionadas a como um módulo é criado para um melhor desempenho.

## <a name="module-manifest-authoring"></a>Criação de manifesto de módulo

Um manifesto de módulo que não use as diretrizes a seguir pode ter um impacto significativo no desempenho geral do PowerShell, mesmo se o módulo não é usado em uma sessão.

Descoberta automática de comando analisa cada módulo para determinar quais comandos o módulo exporta, e essa análise pode ser cara.
Os resultados da análise do módulo são armazenados em cache por usuário, mas o cache não está disponível na primeira execução, o que é um cenário típico com contêineres.
Durante a análise do módulo, se os comandos exportados podem ser determinados completamente do manifesto de análise mais caro do módulo pode ser evitado.

### <a name="guidelines"></a>Diretrizes

* No manifesto do módulo, não use caracteres curinga na `AliasesToExport`, `CmdletsToExport`, e `FunctionsToExport` entradas.

* Se o módulo não exporta os comandos de um determinado tipo, especificar isso explicitamente no manifesto especificando `@()`.
A ausência de um ou `$null` entrada é equivalente a especificar o caractere curinga `*`.

O exemplo a seguir deve ser evitado sempre que possível:

```PowerShell
@{
    FunctionsToExport = '*'

    # Also avoid omitting an entry, it is equivalent to using a wildcard
    # CmdletsToExport = '*'
    # AliasesToExport = '*'
}
```

Em vez disso, use:

```PowerShell
@{
    FunctionsToExport = 'Format-Hex', 'Format-Octal'
    CmdletsToExport = @()  # Specify an empty array, not $null
    AliasesToExport = @()  # Also ensure all three entries are present
}
```

## <a name="avoid-cdxml"></a>Avoid CDXML

Ao decidir como implementar seu módulo, há três opções principais:

* Binário (geralmente C#)
* Script (PowerShell)
* CDXML (um arquivo XML de encapsulamento CIM)

Se a velocidade de carregamento do seu módulo for importante, CDXML é de aproximadamente uma ordem de magnitude mais lenta do que um módulo binário.

Um módulo binário carrega mais rapidamente porque ele é compilado antes do tempo e pode usar o NGen para JIT compilar uma vez por computador.

Um módulo de script normalmente carrega um pouco mais lentamente do que um módulo binário porque o PowerShell deve analisar o script antes de compilar e executá-lo.

Um módulo CDXML é normalmente muito mais lento do que um módulo de script, porque ele deve primeiro analisar um arquivo XML que, em seguida, gera uma grande quantidade de script do PowerShell que é posteriormente analisado e compilado.

