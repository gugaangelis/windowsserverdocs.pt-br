---
title: if
description: Artigo de referência para o comando IF, que executa o processamento condicional em programas do lote.
ms.topic: reference
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea7b823c0060b1fb9ff474ae0330eb789a1da0d1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038004"
---
# <a name="if"></a>if

Executa o processamento condicional em programas do lote.

## <a name="syntax"></a>Sintaxe

```
if [not] ERRORLEVEL <number> <command> [else <expression>]
if [not] <string1>==<string2> <command> [else <expression>]
if [not] exist <filename> <command> [else <expression>]
```

Se as extensões de comando estiverem habilitadas, use a seguinte sintaxe:

```
if [/i] <string1> <compareop> <string2> <command> [else <expression>]
if cmdextversion <number> <command> [else <expression>]
if defined <variable> <command> [else <expression>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |------------ |
| not | Especifica que o comando deve ser executado somente se a condição for falsa. |
| ERRORLEVEL `<number>` | Especifica uma condição verdadeira somente se o programa anterior for executado por Cmd.exe retornou um código de saída igual ou maior que o *número*. |
| `<command>` | Especifica o comando que deve ser executado se a condição anterior for atendida. |
| `<string1>==<string2>` | Especifica uma condição verdadeira somente se *seqüência1* e *seqüência2* forem iguais. Esses valores podem ser cadeias de caracteres literais ou variáveis de lote (por exemplo, `%1` ). Você não precisa colocar cadeias de caracteres literais entre aspas. |
| existentes `<filename>` | Especifica uma condição verdadeira se o nome de arquivo especificado existir. |
| `<compareop>` | Especifica um operador de comparação de três letras, incluindo:<ul><li>**Equ** -igual a</li><li>**NEQ** -não é igual a</li><li>**LSS** -menor que</li><li>**Leq** -menor que ou igual a</li><li>**GTR** -maior que</li><li>**GEQ** -maior ou igual a</li></ul> |
| /i | Força comparações de cadeia de caracteres a ignorar maiúsculas e minúsculas Você pode usar **/i** na `string1==string2` forma de **If**. Essas comparações são genéricas, pois se *seqüência1* e *seqüência2* forem compostas apenas de dígitos numéricos, as cadeias de caracteres serão convertidas em números e uma comparação numérica será executada. |
| cmdextversion `<number>` | Especifica uma condição verdadeira somente se o número de versão interno associado ao recurso de extensões de comando de Cmd.exe for igual ou maior que o número especificado. A primeira versão é 1. Ele aumenta em incrementos de um quando aprimoramentos significativos são adicionados às extensões de comando. A **CMDEXTVERSION** condicional nunca é verdadeira quando as extensões de comando estão desabilitadas (por padrão, as extensões de comando estão habilitadas). |
| defined `<variable>` | Especifica uma condição verdadeira se a *variável* for definida. |
| `<expression>` | Especifica um comando de linha de comando e quaisquer parâmetros a serem passados para o comando em uma cláusula **else** . |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se a condição especificada em uma cláusula **If** for true, o comando que segue a condição será executado. Se a condição for falsa, o comando na cláusula **If** será ignorado e o comando executará qualquer comando especificado na cláusula **else** .

- Quando um programa é interrompido, ele retorna um código de saída. Para usar códigos de saída como condições, use o parâmetro **ERRORLEVEL** .

- Se você usar **definido**, as três variáveis a seguir serão adicionadas ao ambiente: **% ERRORLEVEL%**, **% CMDCMDLINE%** e **% CMDEXTVERSION%**.

  - **% ERRORLEVEL%**: expande para uma representação de cadeia de caracteres do valor atual da variável de ambiente ERRORLEVEL. Essa variável pressupõe que já não existe uma variável de ambiente existente com o nome ERRORLEVEL. Se houver, você obterá o valor ERRORLEVEL em vez disso.

  - **% CMDCMDLINE%**: expande a linha de comando original que foi passada para Cmd.exe antes de qualquer processamento por Cmd.exe. Isso pressupõe que já não existe uma variável de ambiente existente com o nome CMDCMDLINE. Se houver, você obterá o valor de CMDCMDLINE em vez disso.

  - **% CMDEXTVERSION%**: expande para a representação de cadeia de caracteres do valor atual de **CMDEXTVERSION**. Isso pressupõe que já não existe uma variável de ambiente existente com o nome CMDEXTVERSION. Se houver, você obterá o valor de CMDEXTVERSION em vez disso.

- Você deve usar a cláusula **else** na mesma linha que o comando depois de **If**.

### <a name="examples"></a>Exemplos

Para exibir a mensagem **não é possível localizar o arquivo de dados se o arquivo Product. dat não puder ser encontrado**, digite:

```
if not exist product.dat echo Cannot find data file
```

Para formatar um disco na unidade A e exibir uma mensagem de erro se ocorrer um erro durante o processo de formatação, digite as seguintes linhas em um arquivo em lotes:

```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```

Para excluir o arquivo Product. dat do diretório atual ou exibir uma mensagem se Product. dat não for encontrado, digite as seguintes linhas em um arquivo em lotes:

```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> Essas linhas podem ser combinadas em uma única linha da seguinte maneira:
> ```
> IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
> ```

Para ecoar o valor da variável de ambiente ERRORLEVEL depois de executar um arquivo em lotes, digite as seguintes linhas no arquivo em lotes:

```
goto answer%errorlevel%
:answer1
echo The program returned error level 1
goto end
:answer0
echo The program returned error level 0
goto end
:end
echo Done!
```

Para ir para o rótulo ok se o valor da variável de ambiente ERRORLEVEL for menor ou igual a 1, digite:

```
if %errorlevel% LEQ 1 goto okay
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando goto](goto.md)
