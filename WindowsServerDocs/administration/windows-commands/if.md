---
title: if
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2fafd14b0b620a8b5630c869e33c5dbc7cd902f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869227"
---
# <a name="if"></a>if



Executa processamento condicional em programas em lotes.

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

|Parâmetro|Descrição|
|---------|-----------|
|not|Especifica que o comando deve ser executado somente se a condição for falsa.|
|ERRORLEVEL \<número >|Especifica uma condição verdadeira somente se o programa anterior executado por Cmd.exe retornou um código de saída igual ou maior que *número*.|
|\<Comando >|Especifica o comando que deve ser executado se a condição anterior for atendida.|
|\<String1>==<String2>|Especifica uma condição verdadeira somente *String1* e *String2* são os mesmos. Esses valores podem ser cadeias de caracteres literais ou variáveis de lote (por exemplo, %1). Você não precisa colocar cadeias de caracteres literais entre aspas.|
|Existem \<FileName >|Especifica uma condição verdadeira se o nome de arquivo especificado existir.|
|\<CompareOp>|Especifica um operador de comparação de três letras. A lista a seguir representa os valores válidos para *Operador_de_comparação*:</br>**EQU** igual a</br>**EQ** não é igual a</br>**LSS** menor que</br>**LEQ** menor ou igual a</br>**GTR** maior que</br>**GEQ** maior que ou igual a|
|/i|Força as comparações para Ignorar maiusculas e minúsculas da cadeia de caracteres.  Você pode usar **/i** sobre o *String1***==*** String2* forma de **se**. Essas comparações são genéricas, ou se os dois *String1* e *String2* consistem em dígitos numéricos apenas, as cadeias de caracteres são convertidas em números e uma comparação numérica é executada.|
|cmdextversion \<número >|Especifica o número de versão interno associado com as extensões de comando, o recurso de Cmd.exe é igual a ou maior que o número especificado de somente-se uma condição verdadeira. A primeira versão é 1. Ele aumenta em incrementos de um quando aprimoramentos significativos são adicionados às extensões de comando. O **cmdextversion** condicional é verdadeira nunca ao comando extensões estão desabilitadas (por padrão, o comando extensões estão habilitadas).|
|definido \<variável >|Especifica uma condição true se *variável* está definido.|
|\<Expression>|Especifica uma linha de comando e os parâmetros a serem passados para o comando em uma **else** cláusula.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Se a condição especificada em uma **se** cláusula for true, o comando a seguir a condição é executado. Se a condição for falsa, o comando a **se** cláusula será ignorada e o comando executa qualquer comando que é especificado na **else** cláusula.
-   Quando um programa é interrompido, ele retorna um código de saída. Para usar códigos de saída como condições, use **errorlevel**.
-   Se você usar **definidos**, as três variáveis a seguir são adicionadas ao ambiente: **% errorlevel %**, **% cmdcmdline %**, e **% cmdextversion %**.  
    -   **% errorlevel %** expande uma representação de cadeia de caracteres do valor atual da variável de ambiente ERRORLEVEL. Isso pressupõe que não há uma variável de ambiente com o nome ERRORLEVEL — se não houver, você obterá esse valor ERRORLEVEL em vez disso.
    -   **% cmdcmdline %** expande a linha de comando original que foi passada para Cmd.exe antes de qualquer processamento pelo Cmd.exe. Isso pressupõe que não há uma variável de ambiente com o nome CMDCMDLINE — se não houver, você obterá o valor CMDCMDLINE em vez disso.
    -   **% cmdextversion** expande para a representação de cadeia de caracteres do valor atual do **cmdextversion**. Isso pressupõe que não há uma variável de ambiente com o nome CMDEXTVERSION — se não houver, você obterá o valor CMDEXTVERSION em vez disso.
-   Você deve usar o **else** cláusula na mesma linha de comando após a **se**.

## <a name="BKMK_examples"></a>Exemplos

Para exibir a mensagem "Não é possível localizar o arquivo de dados" se o arquivo DAT não for encontrado, digite:
```
if not exist product.dat echo Cannot find data file 
```
Para formatar um disco na unidade e exibir uma mensagem de erro se ocorrer um erro durante o processo de formatação, digite as seguintes linhas em um arquivo em lotes:
```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```
Para excluir o arquivo DAT do diretório atual ou exibir uma mensagem se DAT não for encontrado, digite as seguintes linhas em um arquivo em lotes:
```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> Essas linhas podem ser combinadas em uma única linha da seguinte maneira:
```
IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
```
Para recuperar o valor da variável de ambiente ERRORLEVEL depois de executar um arquivo em lotes, digite as seguintes linhas no arquivo de lote:
```
goto answer%errorlevel%
:answer1
echo Program had return code 1
:answer0
echo Program had return code 0
goto end
:end
echo Done! 
```
Para ir para o rótulo "okey" se o valor da variável de ambiente ERRORLEVEL é menor que ou igual a 1, digite:
```
if %errorlevel% LEQ 1 goto okay
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

[If](if.md)

[Goto](goto.md)