---
title: Considerações de desempenho de script do PowerShell
description: Scripts de desempenho no PowerShell
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: jasonsh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: f22a4f1ba5c0f048e2aa01c744feb3b2b83007a0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851919"
---
# <a name="powershell-scripting-performance-considerations"></a>Considerações de desempenho de script do PowerShell

Os scripts do PowerShell que aproveitam o .NET diretamente e evitam que o pipeline tende a ser mais rápido do que o idiomática PowerShell. O idiomática PowerShell normalmente usa cmdlets e funções do PowerShell de uma grande freqüência, aproveitando o pipeline e descartando o .NET somente quando necessário.

>[!Note] 
> Muitas das técnicas descritas aqui não são idiomática PowerShell e podem reduzir a legibilidade de um script do PowerShell. Autores de script são aconselhados a usar o idiomática PowerShell, a menos que o desempenho dite de outra forma.

## <a name="suppressing-output"></a>Suprimindo saída

Há várias maneiras de evitar a gravação de objetos no pipeline:

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

A atribuição para `$null` ou a conversão para `[void]` são aproximadamente equivalentes e, em geral, devem ser preferenciais onde o desempenho é importante.

```PowerShell
$arrayList.Add($item) > $null
```

O redirecionamento de arquivo para `$null` é quase tão bom quanto as alternativas anteriores, a maioria dos scripts nunca perceberia a diferença.
Dependendo do cenário, o redirecionamento de arquivo apresenta um pouco de sobrecarga no entanto.

```PowerShell
$arrayList.Add($item) | Out-Null
```

O encanamento para `Out-Null` tem sobrecarga significativa quando comparado às alternativas.
Ele deve ser evitado no código sensível ao desempenho.

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

Introduzir um bloco de script e chamá-lo (usando o ponto de origem ou de outra forma) atribuindo o resultado a `$null` é uma técnica conveniente para suprimir a saída de um grande bloco de script.
Essa técnica é executada aproximadamente, bem como tubulação a `Out-Null` e deve ser evitada em script de desempenho sensível.
A sobrecarga extra neste exemplo vem da criação e invocação de um bloco de script que estava anteriormente embutido em script.


## <a name="array-addition"></a>Adição de matriz

A geração de uma lista de itens geralmente é feita usando uma matriz com o operador de adição:

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

Isso pode ser muito inefficent porque as matrizes são imutáveis.
Cada adição à matriz, na verdade, cria uma nova matriz grande o suficiente para conter todos os elementos dos operandos esquerdo e direito e, em seguida, copia os elementos dos dois operandos para a nova matriz.
Para pequenas coleções, essa sobrecarga pode não ser importante.
Para grandes coleções, isso definitivamente pode ser um problema.

Há algumas alternativas.
Se você não precisar realmente de uma matriz, considere usar uma ArrayList:

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

Se você precisar de uma matriz, poderá usar sua própria `ArrayList` e simplesmente chamar `ArrayList.ToArray` quando desejar a matriz.
Como alternativa, você pode permitir que o PowerShell crie o `ArrayList` e `Array` para você:

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

Neste exemplo, o PowerShell cria um `ArrayList` para armazenar os resultados gravados no pipeline dentro da expressão de matriz.
Logo antes de atribuir ao `$results`, o PowerShell converte o `ArrayList` em um `object[]`.

## <a name="processing-large-files"></a>Processando arquivos grandes

A maneira idiomática de processar um arquivo no PowerShell pode ser semelhante a:

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

Isso pode ser quase uma ordem de magnitude mais lenta do que usar as APIs do .NET diretamente:

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

## <a name="avoid-write-host"></a>Evitar write-host

Geralmente, é considerada uma prática ruim escrever a saída diretamente no console, mas quando faz sentido, muitos scripts usam `Write-Host`.

Se você precisar gravar muitas mensagens no console, `Write-Host` pode ser uma ordem de magnitude mais lenta do que `[Console]::WriteLine()`. No entanto, lembre-se de que `[Console]::WriteLine()` é apenas uma alternativa adequada para hosts específicos como PowerShell. exe ou powershell_ise. exe-não há garantia de que funcionem em todos os hosts.

Em vez de usar `Write-Host`, considere o uso de [Write-Output](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1).

