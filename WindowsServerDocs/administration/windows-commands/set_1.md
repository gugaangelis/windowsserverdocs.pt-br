---
title: set
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fdd60d6-addf-4574-8c92-8aa53fa73d76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ece1581bad3d78add93a5bac2c4331ebc5240eef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882127"
---
# <a name="set"></a>set



Exibe, define ou remove o CMD. Variáveis de ambiente do EXE. Se usado sem parâmetros, **definir** exibe as configurações atuais de variáveis de ambiente.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
set [<Variable>=[<String>]]
set [/p] <Variable>=[<PromptString>]
set /a <Variable>=<Expression>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Variable>|Especifica a variável de ambiente para definir ou modificar.|
|\<cadeia de caracteres >|Especifica a cadeia de caracteres a ser associado com a variável de ambiente especificado.|
|/p|Define o valor de *variável* para uma linha de entrada inserida pelo usuário.|
|\<PromptString >|Opcional. Especifica uma mensagem para solicitar a entrada do usuário. Esse parâmetro é usado com o **/p** opção de linha de comando.|
|/a|Conjuntos *cadeia de caracteres* como uma expressão numérica que é avaliada.|
|\<Expression>|Especifica uma expressão numérica. Consulte os comentários para os operadores válidos que podem ser usados em *expressão*.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Usando o **definir** com extensões de comando habilitadas

    Quando as extensões de comando estão habilitadas (padrão) e você executar **definir** com um valor, ele exibe todas as variáveis que começam com esse valor.
-   Usando caracteres especiais

    Os caracteres **<**, **>**, **|**, **&**, **^** são caracteres de shell de comando especial, e que devem ser precedidas pelo caractere de escape (**^**) ou entre aspas quando usado em *decadeiadecaracteres* (por exemplo, **"Sequência contendo & símbolo"**). Se você usar aspas para delimitar uma cadeia de caracteres que contém um dos caracteres especiais, as aspas são definidas como parte do valor de variável de ambiente.
-   Usando variáveis de ambiente

    Use variáveis de ambiente para controlar o comportamento de alguns programas e arquivos em lotes e controlar o Windows de forma e o subsistema do MS-DOS é exibido e funciona. O **definir** comando geralmente é usado no arquivo Autoexec. NT para definir variáveis de ambiente.
-   Exibindo as configurações do ambiente atual

    Quando você digita o **definir** comando isoladamente, as configurações do ambiente atual são exibidas. Geralmente, essas configurações incluem as variáveis de ambiente COMSPEC e PATH, que são usadas para ajudar a localizar programas no disco. Duas outras variáveis de ambiente usadas pelo Windows são PROMPT e DIRCMD.
-   Usando parâmetros

    Quando você especifica valores para *variável* e *cadeia de caracteres*, especificada *variável* valor é adicionado ao ambiente e *cadeia de caracteres* é associado a essa variável. Se a variável já existir no ambiente, o novo valor de cadeia de caracteres substitui o antigo valor de cadeia de caracteres.

    Se você especificar somente uma variável e um sinal de igual (sem *cadeia de caracteres*) para o **definir** comando, o *cadeia de caracteres* valor associado com a variável estiver desmarcada (como se a variável não existe).
-   Usando **/a**

    A tabela a seguir lista os operadores com suporte para **/a** em ordem decrescente de precedência.  
    |Operador|Operação executada|
    |--------|-------------------|
    |( )|Agrupamento|
    |! ~ -|Unários|
    |* / %|aritmética|
    |+ -|aritmética|
    |<< >>|Deslocamento lógico|
    |&|AND bit a bit|
    |^|Bit a bit OR exclusivo|
    |||OR bit a bit|
    |= *= /= %= += -= &= ^= |= <<= >>=|Atribuição|
    |,|Separador de expressões|

    Se você usar lógica (**&&** ou **||**) ou um módulo (**%**) operadores, coloque a cadeia de caracteres de expressão entre aspas. Qualquer cadeia de caracteres não numéricos na expressão é consideradas nomes de variáveis de ambiente e seus valores são convertidos em números antes que eles são processados. Se você especificar um nome de variável de ambiente que não está definido no ambiente atual, um valor de zero é alocado, que permite que você execute aritmética com valores de variáveis de ambiente sem usar o % para recuperar um valor.

    Se você executar **set /a** da linha de comando fora de um script de comando, ele exibe o valor final da expressão.

    Valores numéricos são números decimais, a menos que prefixado por 0 × para números hexadecimais ou 0 para números octais. Portanto, 0 × 12 é o mesmo como 18, que é o mesmo que 022.
-   Suporte a expansão de variáveis de ambiente atrasada

    Suporte à expansão de variáveis de ambiente atrasada está desabilitada por padrão, mas você pode habilitar ou desabilitá-lo usando **cmd /v**.
-   Trabalhando com as extensões de comando

    Quando as extensões de comando estão habilitadas (padrão) e você executar **definir** sozinho, ele exibe todas as variáveis de ambiente atual. Se você executar **definir** com um valor, ele exibe as variáveis que correspondem ao valor.
-   Usando o **definir** em arquivos em lotes

    Ao criar arquivos em lotes, você pode usar **definir** para criar variáveis e, em seguida, usá-los da mesma forma que você usaria as variáveis numeradas **%0** por meio do **%9**. Você também pode usar as variáveis **%0** por meio **%9** como entrada para o **definir**.
-   Chamar um **definir** variável de um arquivo em lotes

    Quando você chama um valor de variável de um arquivo em lotes, coloque o valor entre sinais de porcentagem (**%**). Por exemplo, se seu programa de lote cria uma variável de ambiente denominada BAUD, você pode usar a cadeia de caracteres associada a BAUD como um parâmetro de substituição digitando **% baud %** no prompt de comando.
-   Usando o **definir** no Console de recuperação

    O **definir** comando com parâmetros diferentes, está disponível no Console de recuperação.

## <a name="BKMK_examples"></a>Exemplos

Para definir uma variável de ambiente chamada TEST ^ 1, digite:
```
set testVar=test^^1
```

> [!NOTE]
> O **definir** comando atribui tudo o que segue o sinal de igual (=) para o valor da variável. Se você digitar:
```
set testVar="test^1"
```
Você obterá o seguinte resultado:
```
testVar="test^1"
```
Para definir uma variável de ambiente denominada teste & 1, digite:
```
set testVar=test^&1
```
Para definir uma variável de ambiente denominada INCLUDE para que a cadeia de caracteres C:\Inc (o diretório \Inc na unidade C) é associada a ele, digite:
```
set include=c:\inc
```
Você pode usar a cadeia de caracteres C:\Inc em arquivos em lotes colocando o nome INCLUDE entre sinais de porcentagem (**%**). Por exemplo, você pode incluir o seguinte comando em um arquivo em lotes para que você possa exibir o conteúdo do diretório que está associado com a variável de ambiente INCLUDE:
```
dir %include%
```
Quando esse comando é processado, a cadeia de caracteres C:\Inc substitui **% include %**.

Você também pode usar **definir** em um arquivo em lotes que adiciona um novo diretório para a variável de ambiente PATH. Por exemplo:
```
@echo off
rem ADDPATH.BAT adds a new directory
rem to the path environment variable.
set path=%1;%path%
set
```
Para exibir uma lista de todas as variáveis de ambiente que começam com a letra P, digite:
```
set p 
```

> [!NOTE]
> Este comando requer que as extensões de comando, que são habilitadas por padrão.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)