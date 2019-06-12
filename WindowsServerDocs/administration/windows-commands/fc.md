---
title: fc
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f7ccc3d268b58bfa5e848f2336f4315baae8b4e1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439314"
---
# <a name="fc"></a>fc



Compara dois arquivos ou conjuntos de arquivos e exibe as diferenças entre eles.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fc /a [/c] [/l] [/lb<N>] [/n] [/off[line]] [/t] [/u] [/w] [/<NNNN>] [<Drive1>:][<Path1>]<FileName1> [<Drive2>:][<Path2>]<FileName2>
fc /b [<Drive1:>][<Path1>]<FileName1> [<Drive2:>][<Path2>]<FileName2>
```

## <a name="parameters"></a>Parâmetros

|            Parâmetro             |                                                                                                                                     Descrição                                                                                                                                      |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                /a                |                                                 Abrevia a saída de uma comparação ASCII. Em vez de exibir todas as linhas que são diferentes, **fc** exibe somente a primeira e a última linha para cada conjunto de diferenças.                                                  |
|                /b                |             Compara os dois arquivos no modo binário, byte por byte e não tenta ressincronizar os arquivos depois de localizar uma incompatibilidade. Este é o modo padrão para comparar arquivos que têm as seguintes extensões: .exe,. com,. sys,. obj,. lib ou. bin.              |
|                /c                |                                                                                                                               Ignora a diferenciar maiusculas de minúsculas.                                                                                                                               |
|                /l                |               Compara os arquivos no modo ASCII, linha por linha e tenta ressincronizar os arquivos depois de localizar uma incompatibilidade. Este é o modo padrão para comparar arquivos, exceto os arquivos com as seguintes extensões de arquivo: .exe,. com,. sys,. obj,. lib ou. bin.                |
|             /lb\<N>              |                         Define o número de linhas para o buffer de linha interna *N*. O comprimento padrão do buffer de linha é de 100 linhas. Se os arquivos que estão sendo comparados tiverem mais de 100 linhas consecutivas diferentes, **fc** cancela a comparação.                         |
|                /n                |                                                                                                                Exibe os números de linha durante uma comparação ASCII.                                                                                                                 |
|            /off[line]            |                                                                                                               Não ignorar arquivos que têm o atributo offline definido.                                                                                                               |
|                /t                |                                                                    Impede **fc** converta tabulações em espaços. O comportamento padrão é tratar guias como espaços, com paradas a cada oito posições de caracteres.                                                                    |
|                /u                |                                                                                                                        Compara os arquivos como arquivos de texto Unicode.                                                                                                                         |
|                /w                |         Compacta o espaço em branco (ou seja, tabulações e espaços) durante a comparação. Se uma linha contiver muitos guias, ou espaços consecutivos **/w** trata esses caracteres como um único espaço. Quando usado com **/w**, **fc** ignora o espaço em branco no início e no final de uma linha.         |
|             /\<NNNN>             | Especifica o número de linhas consecutivas que devem corresponder a seguir uma incompatibilidade antes **fc** considera os arquivos sejam ressincronizados. Se o número de linhas correspondentes nos arquivos for menor que *NNNN*, **fc** exibirá as linhas correspondentes como diferenças. O valor padrão é 2. |
| [\<Drive1>:][<Path1>]<FileName1> |                                                                                        Especifica o local e o nome do primeiro arquivo ou conjunto de arquivos a ser comparado. *Arquivo1* é necessária.                                                                                        |
| [\<Drive2>:][<Path2>]<FileName2> |                                                                                       Especifica o local e o nome do segundo arquivo ou conjunto de arquivos a ser comparado. *FileName2* é necessária.                                                                                        |
|                /?                |                                                                                                                         Exibe a ajuda no prompt de comando.                                                                                                                         |

## <a name="remarks"></a>Comentários

-   Esse comando é implemeted por c:\WINDOWS\fc.exe. Você pode usar esse comando no PowerShell, mas certifique-se de soletrar o executável completo (fc.exe), pois 'fc' é um alias para Format-Custom.

-   Relatório de diferenças entre arquivos em uma comparação de ASCII

    Quando você usa **fc** para uma comparação de ASCII **fc** exibe as diferenças entre dois arquivos na seguinte ordem:  
    -   Nome do primeiro arquivo
    -   Linhas de *arquivo1* que diferem entre os arquivos
    -   Primeira linha correspondente em ambos os arquivos
    -   Nome do segundo arquivo
    -   Linhas de *FileName2* que diferem
    -   Primeira linha correspondente
-   Usando o **/b** para comparações binárias

    **/ b** exibe incompatibilidades que são encontradas durante uma comparação binária na sintaxe a seguir:

    `\<XXXXXXXX: YY ZZ>`

    O valor de *XXXXXXXX* Especifica o endereço relativo hexadecimal para o par de bytes, medido desde o início do arquivo. Os endereços iniciam em 00000000. O hexadecimal valores para *AA* e *ZZ* representam os bytes diferentes de *arquivo1* e *FileName2*, respectivamente.
-   Usando caracteres curinga

    Você pode usar caracteres curinga ( **&#42;** e **?** ) em *arquivo1* e *FileName2*. Se você usar um caractere curinga no *arquivo1*, **fc** compara todos os arquivos especificados para o arquivo ou conjunto de arquivos especificado por *FileName2*. Se você usar um caractere curinga no *FileName2*, **fc** usa o valor correspondente de *arquivo1*.
-   Trabalhando com memória

    Ao comparar arquivos de ASCII **fc** usa um buffer interno (suficientemente grande para conter 100 linhas) como armazenamento. Se os arquivos forem maiores do que o buffer **fc** compara o que pode ser carregado no buffer. Se **fc** não encontra uma correspondência nas partes dos arquivos carregadas, interrupção e exibirá a seguinte mensagem:

    `Resynch failed. Files are too different.`

    Ao comparar arquivos binários que são maiores do que a memória disponível, **fc** compara os dois arquivos completamente, sobrepondo as partes da memória com as partes próxima do disco. A saída é o mesmo que para arquivos que se encaixam completamente na memória.

## <a name="BKMK_examples"></a>Exemplos

Para fazer uma comparação de ASCII de dois arquivos de texto, rpt e vendas. rpt e exibir os resultados no formato abreviado, digite:
```
fc /a monthly.rpt sales.rpt 
```
Para fazer uma comparação binária entre dois arquivos em lotes,. bat e bat, digite:
```
fc /b profits.bat earnings.bat
```
Resultados semelhantes à seguir aparecem:
```
00000002: 72 43
00000004: 65 3A
0000000E: 56 92
...
...
...
000005E8: 00 6E
FC: Earnings.bat longer than Profits.bat
```
Se os arquivos. bat e bat forem idênticos, **fc** exibe a seguinte mensagem:
```
Comparing files Profits.bat and Earnings.bat
FC: no differences encountered
```
Para comparar cada arquivo. bat no diretório atual com o arquivo novo. bat, digite:
```
fc *.bat new.bat
```
Para comparar o arquivo novo. bat na unidade C com o arquivo novo. bat na unidade D, digite:
```
fc c:new.bat d:*.bat
```
Para comparar cada arquivo em lotes no diretório raiz da unidade C para o arquivo com o mesmo nome no diretório raiz da unidade D, digite:
```
fc c:*.bat d:*.bat
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
