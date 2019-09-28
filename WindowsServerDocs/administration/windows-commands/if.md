---
title: if
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd7857251b0b6a943f2eea33f56732ec57e7e8d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375483"
---
# <a name="if"></a>if



Executa o processamento condicional em programas do lote.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
if [not] ERRORLEVEL <Number> <Command> [else <Expression>]
if [not] <String1>==<String2> <Command> [else <Expression>]
if [not] exist <FileName> <Command> [else <Expression>]
```
Se as extensões de comando estiverem habilitadas, use a seguinte sintaxe:
```
if [/i] <String1> <CompareOp> <String2> <Command> [else <Expression>]
if cmdextversion <Number> <Command> [else <Expression>]
if defined <Variable> <Command> [else <Expression>]
```

## <a name="parameters"></a>Parâmetros

|        Parâmetro        |                                                                                                                                                                                                                Descrição                                                                                                                                                                                                                 |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           not           |                                                                                                                                                                              Especifica que o comando deve ser executado somente se a condição for falsa.                                                                                                                                                                              |
|  ERRORLEVEL \<Number >   |                                                                                                                                                      Especifica uma condição verdadeira somente se o programa anterior for executado pelo cmd. exe retornou um código de saída igual ou maior que o *número*.                                                                                                                                                       |
|       \<Command >        |                                                                                                                                                                            Especifica o comando que deve ser executado se a condição anterior for atendida.                                                                                                                                                                             |
|  \<String1 > = = <String2>  |                                                                                                             Especifica uma condição verdadeira somente se *seqüência1* e *seqüência2* forem iguais. Esses valores podem ser cadeias de caracteres literais ou variáveis de lote (por exemplo,% 1). Você não precisa colocar cadeias de caracteres literais entre aspas.                                                                                                              |
|    existe \<FileName >    |                                                                                                                                                                                       Especifica uma condição verdadeira se o nome de arquivo especificado existir.                                                                                                                                                                                        |
|      \<CompareOp >       |                                                                               Especifica um operador de comparação de três letras. A lista a seguir representa valores válidos para *CompareOp*:</br>**Equ** Igual a</br>**NEQ** Diferente de</br>**LSS** Menor que</br>**Leq** Menor ou igual a</br>**GTR** Maior que</br>**GEQ** Maior ou igual a                                                                                |
|           /i            |                                                            Força comparações de cadeia de caracteres a ignorar maiúsculas e minúsculas  Você pode usar **/i** na forma <em>seqüência1</em> **==** <em>seqüência2</em> de **If**. Essas comparações são genéricas, pois se *seqüência1* e *seqüência2* forem compostas apenas de dígitos numéricos, as cadeias de caracteres serão convertidas em números e uma comparação numérica será executada.                                                            |
| CMDEXTVERSION \<Number > | Especifica uma condição verdadeira somente se o número de versão interno associado ao recurso de extensões de comando do cmd. exe for igual ou maior que o número especificado. A primeira versão é 1. Ele aumenta em incrementos de um quando aprimoramentos significativos são adicionados às extensões de comando. A **CMDEXTVERSION** condicional nunca é verdadeira quando as extensões de comando estão desabilitadas (por padrão, as extensões de comando estão habilitadas). |
|   definido \<Variable >   |                                                                                                                                                                                            Especifica uma condição verdadeira se a *variável* for definida.                                                                                                                                                                                            |
|      \<Expression >      |                                                                                                                                                                   Especifica um comando de linha de comando e quaisquer parâmetros a serem passados para o comando em uma cláusula **else** .                                                                                                                                                                   |
|           /?            |                                                                                                                                                                                                    Exibe a ajuda no prompt de comando.                                                                                                                                                                                                    |

## <a name="remarks"></a>Comentários

-   Se a condição especificada em uma cláusula **If** for true, o comando que segue a condição será executado. Se a condição for falsa, o comando na cláusula **If** será ignorado e o comando executará qualquer comando especificado na cláusula **else** .
-   Quando um programa é interrompido, ele retorna um código de saída. Para usar códigos de saída como condições, use **ERRORLEVEL**.
-   Se você usar **definido**, as três variáveis a seguir serão adicionadas ao ambiente: **% ERRORLEVEL%** , **% CMDCMDLINE%** e **% CMDEXTVERSION%** .  
    -   **% ERRORLEVEL%** se expande para uma representação de cadeia de caracteres do valor atual da variável de ambiente ERRORLEVEL. Isso pressupõe que não há uma variável de ambiente existente com o nome ERRORLEVEL — se houver, você obterá esse valor ERRORLEVEL.
    -   **% CMDCMDLINE%** se expande para a linha de comando original que foi passada para cmd. exe antes de qualquer processamento pelo cmd. exe. Isso pressupõe que não há uma variável de ambiente existente com o nome CMDCMDLINE — se houver, você obterá o valor de CMDCMDLINE em vez disso.
    -   **% CMDEXTVERSION%** se expande para a representação de cadeia de caracteres do valor atual de **CMDEXTVERSION**. Isso pressupõe que não há uma variável de ambiente existente com o nome CMDEXTVERSION — se houver, você obterá o valor de CMDEXTVERSION em vez disso.
-   Você deve usar a cláusula **else** na mesma linha que o comando depois de **If**.

## <a name="BKMK_examples"></a>Disso

Para exibir a mensagem "não é possível localizar o arquivo de dados" se o arquivo Product. dat não puder ser encontrado, digite:
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
> Para ecoar o valor da variável de ambiente ERRORLEVEL depois de executar um arquivo em lotes, digite as seguintes linhas no arquivo em lotes:
> ```
> goto answer%errorlevel%
> :answer1
> echo Program had return code 1
> :answer0
> echo Program had return code 0
> goto end
> :end
> echo Done! 
> ```
> Para ir para o rótulo "OK" se o valor da variável de ambiente ERRORLEVEL for menor ou igual a 1, digite:
> ```
> if %errorlevel% LEQ 1 goto okay
> ```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Que](if.md)

[Fim](goto.md)