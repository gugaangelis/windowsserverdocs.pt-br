---
title: fc
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 372e8b6a605bd96f6287a005004fd2f1532dfe4f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725642"
---
# <a name="fc"></a>fc



Compara dois arquivos ou conjuntos de arquivos e exibe as diferenças entre eles.



## <a name="syntax"></a>Sintaxe

```
fc /a [/c] [/l] [/lb<N>] [/n] [/off[line]] [/t] [/u] [/w] [/<NNNN>] [<Drive1>:][<Path1>]<FileName1> [<Drive2>:][<Path2>]<FileName2>
fc /b [<Drive1:>][<Path1>]<FileName1> [<Drive2:>][<Path2>]<FileName2>
```

### <a name="parameters"></a>Parâmetros

|            Parâmetro             |                                                                                                                                     Descrição                                                                                                                                      |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                /a                |                                                 Abrevia a saída de uma comparação ASCII. Em vez de exibir todas as linhas que são diferentes, o **FC** exibe apenas a primeira e a última linha para cada conjunto de diferenças.                                                  |
|                /b                |             Compara os dois arquivos no modo binário, byte por byte, e não tenta ressincronizar os arquivos depois de encontrar uma incompatibilidade. Este é o modo padrão para comparar arquivos que têm as seguintes extensões de arquivo:. exe,. com,. sys,. obj,. lib ou. bin.              |
|                /c                |                                                                                                                               Ignora o caso de letra.                                                                                                                               |
|                /l                |               Compara os arquivos no modo ASCII, linha por linha, e tenta ressincronizar os arquivos depois de encontrar uma incompatibilidade. Esse é o modo padrão para comparar arquivos, exceto arquivos com as seguintes extensões de arquivo:. exe,. com,. sys,. obj,. lib ou. bin.                |
|             /lb\<N>              |                         Define o número de linhas para o buffer de linha interno como *N*. O comprimento padrão do buffer de linha é de 100 linhas. Se os arquivos que você está comparando tiverem mais de 100 linhas de diferença consecutivas, o **FC** cancelará a comparação.                         |
|                /n                |                                                                                                                Exibe os números de linha durante uma comparação ASCII.                                                                                                                 |
|            /off [linha]            |                                                                                                               Não ignora arquivos que têm o atributo offline definido.                                                                                                               |
|                /t                |                                                                    Impede que o **FC** converta tabulações em espaços. O comportamento padrão é tratar as guias como espaços, com paradas em cada oitavo posição de caractere.                                                                    |
|                /u                |                                                                                                                        Compara arquivos como arquivos de texto Unicode.                                                                                                                         |
|                /w                |         Compacta o espaço em branco (ou seja, tabulações e espaços) durante a comparação. Se uma linha contiver muitos espaços ou guias consecutivos, **/w** tratará esses caracteres como um único espaço. Quando usado com **/w**, o **FC** ignora o espaço em branco no início e no fim de uma linha.         |
|             /\<NNNN>             | Especifica o número de linhas consecutivas que devem corresponder após uma incompatibilidade, antes de o **FC** considerar os arquivos a serem ressincronizados. Se o número de linhas correspondentes nos arquivos for menor que *nnnn*, o **FC** exibirá as linhas correspondentes como diferenças. O valor padrão é 2. |
| [\<Unidade1>:] [<Path1>]<FileName1> |                                                                                        Especifica o local e o nome do primeiro arquivo ou conjunto de arquivos a ser comparado. *Arquivo1* é necessário.                                                                                        |
| [\<Unidade2>:] [<Path2>]<FileName2> |                                                                                       Especifica o local e o nome do segundo arquivo ou conjunto de arquivos a ser comparado. *Arquivo2* é necessário.                                                                                        |
|                /?                |                                                                                                                         Exibe a ajuda no prompt de comando.                                                                                                                         |

## <a name="remarks"></a>Comentários

-   Este comando é implemeted por c:\WINDOWS\fc.exe. Você pode usar esse comando no PowerShell, mas certifique-se de soletrar o executável completo (FC. exe), pois "FC" é um alias para Format-Custom.

-   Relatando diferenças entre arquivos para uma comparação ASCII

    Quando você usa o **FC** para uma comparação ASCII, o **FC** exibe as diferenças entre dois arquivos na seguinte ordem:  
    -   Nome do primeiro arquivo
    -   Linhas de *arquivo1* que diferem entre os arquivos
    -   Primeira linha para corresponder em ambos os arquivos
    -   Nome do segundo arquivo
    -   Linhas de *arquivo2* que diferem
    -   Primeira linha para correspondência
-   Usando **/b** para comparações binárias

    **/b** exibe incompatibilidades encontradas durante uma comparação binária na seguinte sintaxe:

    `\<XXXXXXXX: YY ZZ>`

    O valor *xxxxxxxx* especifica o endereço hexadecimal relativo para o par de bytes, medido desde o início do arquivo. Os endereços começam em 00000000. Os valores hexadecimais de *YY* e *ZZ* representam os bytes incompatíveis de *arquivo1* e *arquivo2*, respectivamente.
-   Usando caracteres curinga

    Você pode usar caracteres curinga (**&#42;** e **?**) em *arquivo1* e *arquivo2*. Se você usar um caractere curinga em *arquivo1*, **FC** comparará todos os arquivos especificados para o arquivo ou conjunto de arquivos especificados por *arquivo2*. Se você usar um caractere curinga em *arquivo2*, **FC** usará o valor correspondente de *arquivo1*.
-   Trabalhando com memória

    Ao comparar arquivos ASCII, o **FC** usa um buffer interno (grande o suficiente para conter 100 linhas) como armazenamento. Se os arquivos forem maiores do que o buffer, o **FC** compara o que ele pode carregar no buffer. Se o **FC** não encontrar uma correspondência nas partes carregadas dos arquivos, ele parará e exibirá a seguinte mensagem:

    `Resynch failed. Files are too different.`

    Ao comparar arquivos binários maiores que a memória disponível, o **FC** compara os arquivos completamente, sobrepondo as partes na memória com as próximas partes do disco. A saída é a mesma que para arquivos que se encaixam completamente na memória.

## <a name="examples"></a>Exemplos

Para fazer uma comparação ASCII de dois arquivos de texto, Monthly. RPT e Sales. RPT e exibir os resultados em formato abreviado, digite:
```
fc /a monthly.rpt sales.rpt 
```
Para fazer uma comparação binária de dois arquivos em lotes, lucros. bat e ganhos. bat, digite:
```
fc /b profits.bat earnings.bat
```
Resultados semelhantes aos seguintes são exibidos:
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
Se os arquivos lucros. bat e ganhos. bat forem idênticos, o **FC** exibirá a seguinte mensagem:
```
Comparing files Profits.bat and Earnings.bat
FC: no differences encountered
```
Para comparar cada arquivo. bat no diretório atual com o arquivo New. bat, digite:
```
fc *.bat new.bat
```
Para comparar o arquivo New. bat na unidade C com o arquivo New. bat na unidade D, digite:
```
fc c:new.bat d:*.bat
```
Para comparar cada arquivo em lotes no diretório raiz na unidade C para o arquivo com o mesmo nome no diretório raiz na unidade D, digite:
```
fc c:*.bat d:*.bat
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
