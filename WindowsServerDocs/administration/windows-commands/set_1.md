---
title: set
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 12dce38bf8ad050c65a7a8c0fca4a71267cca93f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384097"
---
# <a name="set"></a>set



Exibe, define ou remove CMD. Variáveis de ambiente EXE. Se usado sem parâmetros, **set** exibe as configurações de variável de ambiente atuais.

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
|Variável de \<>|Especifica a variável de ambiente a ser definida ou modificada.|
|Cadeia de caracteres \<>|Especifica a cadeia de caracteres a ser associada à variável de ambiente especificada.|
|/p|Define o valor da *variável* para uma linha de entrada inserida pelo usuário.|
|\<Promptstring >|Opcional. Especifica uma mensagem para solicitar a entrada do usuário. Esse parâmetro é usado com a opção de linha de comando **/p** .|
|SRDF|Define a *cadeia de caracteres* para uma expressão numérica que é avaliada.|
|Expressão de \<>|Especifica uma expressão numérica. Consulte comentários para operadores válidos que podem ser usados na *expressão*.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Usando **set** com extensões de comando habilitadas

  Quando as extensões de comando são habilitadas (o padrão) e você executa **definir** com um valor, ele exibe todas as variáveis que começam com esse valor.
- Usando caracteres especiais

  Os caracteres **<** , **>** , **|** , **&** , **^** são caracteres especiais do Shell de comando e devem ser precedidos pelo caractere de escape ( **^** ) ou colocados entre aspas quando usados na *cadeia de caracteres* (por exemplo, **"StringContaining & Symbol"** ). Se você usar aspas para colocar uma cadeia de caracteres que contenha um dos personagens especiais, as aspas serão definidas como parte do valor da variável de ambiente.
- Usando variáveis de ambiente

  Use variáveis de ambiente para controlar o comportamento de alguns arquivos e programas em lotes e para controlar a maneira como o Windows e o subsistema MS-DOS são exibidos e funcionam. O comando **set** é geralmente usado no arquivo autoexec. NT para definir variáveis de ambiente.
- Exibindo as configurações de ambiente atuais

  Quando você digita apenas o comando **set** , as configurações de ambiente atuais são exibidas. Essas configurações geralmente incluem as variáveis de ambiente COMSPEC e PATH, que são usadas para ajudar a localizar programas no disco. Duas outras variáveis de ambiente usadas pelo Windows são PROMPT e DIRCMD.
- Usando parâmetros

  Quando você especifica valores para *variável* e *cadeia de caracteres*, o valor de *variável* especificado é adicionado ao ambiente e a *cadeia de caracteres* é associada a essa variável. Se a variável já existir no ambiente, o novo valor da cadeia de caracteres substituirá o valor antigo da cadeia de caracteres.

  Se você especificar apenas uma variável e um sinal de igual (sem *cadeia de caracteres*) para o comando **set** , o valor da cadeia de *caracteres* associado à variável será limpo (como se a variável não existir).
- Usando **/a**

  A tabela a seguir lista os operadores com suporte para **/a** em ordem decrescente de precedência.  

  |        Operador         | Operação executada  |
  |-------------------------|----------------------|
  |           ( )           |       Agrupamento       |
  |          ! ~ -          |        Unários         |
  |         \*/%          |      Operações      |
  |           + -           |      Operações      |
  |          < < > >          |    Deslocamento lógico     |
  |            &            |     E      |
  |            ^            | Or exclusivo de or |
  |                         |                      |
  | = \*=/=% = + =-= & = ^ = |      = < < = > > =       |
  |            ,            | Separador de expressão |

  Se você usar operadores lógicos ( **&&** ou **||** ) ou de módulo ( **%** ), coloque a cadeia de caracteres de expressão entre aspas. Todas as cadeias de caracteres não numéricas na expressão são consideradas nomes de variáveis de ambiente e seus valores são convertidos em números antes de serem processados. Se você especificar um nome de variável de ambiente que não esteja definido no ambiente atual, um valor de zero será alocado, o que permitirá que você execute aritmética com valores de variáveis de ambiente sem usar% para recuperar um valor.

  Se você executar **set/a** na linha de comando fora de um script de comando, ele exibirá o valor final da expressão.

  Os valores numéricos são números decimais, a menos que sejam prefixados por 0 × para números hexadecimais ou 0 para números octais. Portanto, 0 × 12 é o mesmo que 18, que é o mesmo que 022.
- Suporte à expansão de variável de ambiente atrasada

  O suporte à expansão de variável de ambiente atrasada está desabilitado por padrão, mas você pode habilitá-lo ou desabilitá-lo usando **cmd/v**.
- Trabalhando com extensões de comando

  Quando as extensões de comando são habilitadas (o padrão) e você executa somente **definido** , ele exibe todas as variáveis de ambiente atuais. Se você executar **set** com um valor, ele exibirá as variáveis que correspondem a esse valor.
- Usando **set** em arquivos em lotes

  Ao criar arquivos em lotes, você pode usar **set** para criar variáveis e, em seguida, usá-las da mesma maneira que usaria as variáveis numeradas **%0** a **%9**. Você também pode usar as variáveis **%0** a **%9** como entrada para o **conjunto**.
- Chamando uma variável **set** de um arquivo em lotes

  Ao chamar um valor de variável de um arquivo em lotes, coloque o valor com sinais de porcentagem ( **%** ). Por exemplo, se o programa do lote criar uma variável de ambiente chamada BAUD, você poderá usar a cadeia de caracteres associada à BAUD como um parâmetro substituível digitando **% baud%** no prompt de comando.
- Usando **set** no console de recuperação

  O comando **set** , com parâmetros diferentes, está disponível no console de recuperação.

## <a name="BKMK_examples"></a>Disso

Para definir uma variável de ambiente chamada TEST ^ 1, digite:
```
set testVar=test^^1
```

> [!NOTE]
> O comando **set** atribui tudo o que segue o sinal de igual (=) para o valor da variável. Se você digitar:
> ```
> set testVar="test^1"
> ```
> Você Obtém o seguinte resultado:
> ```
> testVar="test^1"
> ```
> Para definir uma variável de ambiente chamada TEST & 1, digite:
> ```
> set testVar=test^&1
> ```
> Para definir uma variável de ambiente denominada INCLUDE para que a cadeia de caracteres C:\Inc (o diretório \Inc na unidade C) esteja associada a ela, digite:
> ```
> set include=c:\inc
> ```
> Em seguida, você pode usar a cadeia de caracteres C:\Inc em arquivos em lotes ao colocar o nome incluir com sinais de porcentagem ( **%** ). Por exemplo, você pode incluir o seguinte comando em um arquivo em lotes para que você possa exibir o conteúdo do diretório que está associado à variável de ambiente INCLUDE:
> ```
> dir %include%
> ```
> Quando esse comando é processado, a cadeia de caracteres C:\Inc substitui **% include%** .

Você também pode usar **set** em um programa em lotes que adiciona um novo diretório à variável de ambiente Path. Por exemplo:
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
> Esse comando requer extensões de comando, que são habilitadas por padrão.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)