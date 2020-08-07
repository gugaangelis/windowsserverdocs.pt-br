---
title: Considerações de criação de módulo do PowerShell
description: Considerações de criação de módulo do PowerShell
ms.topic: article
ms.author: jasonsh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: bb22009262cc1ae713846779c6b24402e3ed7928
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896263"
---
# <a name="powershell-module-authoring-considerations"></a>Considerações de criação de módulo do PowerShell

Este documento inclui algumas diretrizes relacionadas à forma como um módulo é criado para melhor desempenho.

## <a name="module-manifest-authoring"></a>Criação de manifesto de módulo

Um manifesto de módulo que não usa as diretrizes a seguir pode ter um impacto perceptível no desempenho geral do PowerShell, mesmo se o módulo não for usado em uma sessão.

A descoberta automática de comando analisa cada módulo para determinar quais comandos são exportados pelo módulo e essa análise pode ser cara.
Os resultados da análise de módulo são armazenados em cache por usuário, mas o cache não está disponível na primeira execução, que é um cenário típico com contêineres.
Durante a análise do módulo, se os comandos exportados puderem ser totalmente determinados do manifesto, uma análise mais cara do módulo poderá ser evitada.

### <a name="guidelines"></a>Diretrizes

* No manifesto do módulo, não use caracteres curinga nas `AliasesToExport` `CmdletsToExport` entradas, e `FunctionsToExport` .

* Se o módulo não exportar comandos de um tipo específico, especifique explicitamente no manifesto especificando `@()` .
Uma entrada ausente ou `$null` é equivalente à especificação do caractere curinga `*` .

O seguinte deve ser evitado sempre que possível:

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

## <a name="avoid-cdxml"></a>Evitar CDXML

Ao decidir como implementar seu módulo, há três opções principais:

* Binary (geralmente C#)
* Script (PowerShell)
* CDXML (um arquivo XML com encapsulamento CIM)

Se a velocidade de carregar seu módulo for importante, o CDXML é aproximadamente uma ordem de magnitude mais lenta do que um módulo binário.

Um módulo binário carrega o mais rápido porque ele é compilado antecipadamente e pode usar NGen para compilação JIT uma vez por computador.

Um módulo de script normalmente carrega um pouco mais lentamente do que um módulo binário, pois o PowerShell deve analisar o script antes de compilá-lo e executá-lo.

Um módulo CDXML normalmente é muito mais lento do que um módulo de script, pois ele deve primeiro analisar um arquivo XML que, em seguida, gera um pouco de script do PowerShell que é então analisado e compilado.

