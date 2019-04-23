---
title: Considerações de desempenho de script do PowerShell
description: Scripts para o desempenho no PowerShell
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: df406c7e382907ca32006ec4dae6537a140a91cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830367"
---
# <a name="powershell-scripting-performance-considerations"></a>Considerações de desempenho de script do PowerShell

Scripts do PowerShell que aproveitem .NET diretamente e evitar o pipeline tendem a ser mais rápido do que as expressões idiomática PowerShell. Expressões idiomática PowerShell normalmente usa cmdlets do PowerShell as funções e intensamente, geralmente aproveitando o pipeline e soltar para baixo no .NET somente quando necessário.

>[!Note] 
> Muitas das técnicas descritas aqui não são expressões idiomática PowerShell e podem reduzir a legibilidade de um script do PowerShell. Os autores de script são aconselhados a usar o PowerShell expressões idiomática, a menos que o desempenho indique o contrário.

## <a name="suppressing-output"></a>Suprimir saída

Há muitas maneiras de evitar a escrita de objetos para o pipeline:

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

Atribuição para `$null` ou de projeção para `[void]` são praticamente equivalentes e geralmente deve ser preferencial em que o desempenho é importante.

```PowerShell
$arrayList.Add($item) > $null
```

Redirecionamento de arquivo `$null` é quase tão bom quanto as alternativas anteriores, mais scripts nunca Note a diferença.
Dependendo do cenário, o redirecionamento de arquivo introduzir um pouco de sobrecarga no entanto.

```PowerShell
$arrayList.Add($item) | Out-Null
```

Redirecionar para `Out-Null` tem uma sobrecarga significativa em comparação com as alternativas.
Ele deve ser evitando no código confidencial de desempenho.

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

Apresentando um bloco de script e chamá-lo (usando dot sourcing ou de outra forma) e em seguida, atribuir o resultado para `$null` é uma técnica conveniente para suprimir a saída de um grande bloco de script.
Essa técnica aproximadamente executa, bem como o pipe para `Out-Null` e deve ser evitada no script confidenciais de desempenho.
A sobrecarga extra neste exemplo vem da criação e a invocação de um bloco de script que estava anteriormente no script embutido.


## <a name="array-addition"></a>Adição de matriz

Gerando uma lista de itens é geralmente feito usando uma matriz com o operador de adição:

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

Isso pode ser muito inefficent porque as matrizes são imutáveis.
Cada adição à matriz, na verdade, cria uma nova matriz grande o suficiente para conter todos os elementos de ambos os operandos left e right e, em seguida, copia os elementos de ambos os operandos para a nova matriz.
Para coleções pequenas, essa sobrecarga não pode ser importante.
Para coleções grandes, isso definitivamente pode ser um problema.

Há duas alternativas.
Se você realmente não exigir uma matriz, em vez disso, considere usar um ArrayList:

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

Se você precisar de uma matriz, você pode usar seu próprio `ArrayList` e simplesmente chamar `ArrayList.ToArray` quando deseja que a matriz.
Como alternativa, você pode permitir que PowerShell crie o `ArrayList` e `Array` para você:

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

Neste exemplo, o PowerShell cria um `ArrayList` para armazenar os resultados gravados para o pipeline dentro da expressão de matriz.
Antes de atribuí `$results`, PowerShell converte os `ArrayList` para um `object[]`.

## <a name="processing-large-files"></a>Processamento de arquivos grandes

A forma idiomática de processar um arquivo no PowerShell pode ser algo como:

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

Isso pode ser praticamente uma ordem de magnitude mais lenta do que usando as APIs do .NET diretamente:

```PowerShell
try
{
    $stream = [System.IO.StreamReader]::new($path)
    while ($line = $stream.ReadLine())
    {
        if ($line.Length -gt 10)
        {
            $line
        }
    }
}
finally
{
    $stream.Dispose()
}
```

## <a name="avoid-write-host"></a>Evite o Write-Host

É geralmente considerada uma prática inadequada para gravar a saída diretamente para o console, mas quando fizer sentido, muitos scripts usam `Write-Host`.

Se você deve escrever muitas mensagens no console `Write-Host` pode ser mais lento do que uma ordem de magnitude `[Console]::WriteLine()`. No entanto, lembre-se que `[Console]::WriteLine()` é apenas uma alternativa adequada para hosts específicos, como powershell.exe ou powershell_ise.exe – não há garantia de que funcione em todos os hosts.

Em vez de usar `Write-Host`, considere o uso [Write-Output](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1).

