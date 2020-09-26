---
title: set
description: Artigo de referência para Set, que exibe, define ou remove cmd.exe variáveis de ambiente.
ms.topic: reference
ms.assetid: 5fdd60d6-addf-4574-8c92-8aa53fa73d76
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2f53655aa344e1770c9483e5302885734c389837
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389024"
---
# <a name="set-environment-variable"></a>Set (variável de ambiente)

Exibe, define ou remove cmd.exe variáveis de ambiente. Se usado sem parâmetros, **set** exibe as configurações de variável de ambiente atuais.

> [!NOTE]
> Esse comando requer extensões de comando, que são habilitadas por padrão.

O comando **set** também pode ser executado no console de recuperação do Windows, usando parâmetros diferentes. Para obter mais informações, consulte [ambiente de recuperação do Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxe

```
set [<variable>=[<string>]]
set [/p] <variable>=[<promptString>]
set /a <variable>=<expression>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<variable>` | Especifica a variável de ambiente a ser definida ou modificada. |
| `<string>` | Especifica a cadeia de caracteres a ser associada à variável de ambiente especificada. |
| /p | Define o valor de `<variable>` para uma linha de entrada inserida pelo usuário. |
| `<promptstring>` | Especifica uma mensagem para solicitar a entrada do usuário. Esse parâmetro deve ser usado com o parâmetro **/p** . |
| /a | Define `<string>` para uma expressão numérica que é avaliada. |
| `<expression>` | Especifica uma expressão numérica. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se as extensões de comando estiverem habilitadas (o padrão) e você executar **definir** com um valor, ele exibirá todas as variáveis que começam com esse valor.

- Os caracteres `<` , `>` , `|` , `&` e `^` são caracteres especiais do Shell de comando, e eles devem ser precedidos pelo caractere de escape ( `^` ) ou entre aspas quando usados em `<string>` (por exemplo, "StringContaining&Symbol"). Se você usar aspas para incluir uma cadeia de caracteres que contenha um dos caracteres especiais, as aspas serão definidas como parte do valor da variável de ambiente.

- Use variáveis de ambiente para controlar o comportamento de alguns arquivos e programas em lotes e para controlar a maneira como o Windows e o subsistema MS-DOS são exibidos e funcionam. O comando **set** é geralmente usado no arquivo **Autoexec. NT** para definir variáveis de ambiente.

- Se você usar o comando **set** sem parâmetros, as configurações de ambiente atuais serão exibidas. Essas configurações geralmente incluem as variáveis de ambiente **COMSPEC** e **Path** , que são usadas para ajudar a localizar programas no disco. Duas outras variáveis de ambiente usadas pelo Windows são **prompt** e **DIRCMD**.

- Se você especificar valores para `<variable>` e `<string>` , o `<variable>` valor especificado será adicionado ao ambiente e `<string>` será associado a essa variável. Se a variável já existir no ambiente, o novo valor da cadeia de caracteres substituirá o valor antigo da cadeia de caracteres.

- Se você especificar apenas uma variável e um sinal de igual (sem `<string>` ) para o comando **set** , o `<string>` valor associado à variável será limpo (como se a variável não existir).

- Se você usar o parâmetro **/a** , os seguintes operadores têm suporte, em ordem decrescente de precedência:

  | Operador | Operação executada |
  |--|--|
  | `( )` | Agrupamento |
  | `! ~ -` | Unário |
  | `* / %` | Aritmético |
  | `+ -` | Aritmético |
  | `<< >>` | Deslocamento lógico |
  | `&` | AND bit a bit |
  | `^` | OR exclusivo bit a bit |
  | `= *= /= %= += -= &= ^=` | `= <<= >>=` |
  | `,` | Separador de expressão |

- Se você usar operadores lógicos ( `&&` ou `||` ) ou de módulo ( **%** ), coloque a cadeia de caracteres de expressão entre aspas. Todas as cadeias de caracteres não numéricas na expressão são consideradas nomes de variáveis de ambiente e seus valores são convertidos em números antes de serem processados. Se você especificar um nome de variável de ambiente que não esteja definido no ambiente atual, um valor de zero será alocado, o que permitirá que você execute aritmética com valores de variáveis de ambiente sem usar% para recuperar um valor.

- Se você executar **set/a** na linha de comando fora de um script de comando, ele exibirá o valor final da expressão.

- Os valores numéricos são números decimais, a menos que sejam prefixados por 0 × para números hexadecimais ou 0 para números octais. Portanto, 0 × 12 é o mesmo que 18, que é o mesmo que 022.

- O suporte à expansão de variável de ambiente atrasada está desabilitado por padrão, mas você pode habilitá-lo ou desabilitá-lo usando **cmd/v**.

- Ao criar arquivos em lotes, você pode usar **set** para criar variáveis e, em seguida, usá-las da mesma maneira que usaria as variáveis numeradas **%0** a **%9**. Você também pode usar as variáveis **%0** a **%9** como entrada para o **conjunto**.

- Se você chamar um valor de variável de um arquivo em lotes, coloque o valor com sinais de porcentagem ( **%** ). Por exemplo, se o programa do lote criar uma variável de ambiente chamada *baud*, você poderá usar a cadeia de caracteres associada à *baud* como um parâmetro substituível digitando **% baud%** no prompt de comando.

## <a name="examples"></a>Exemplos

Para definir uma variável de ambiente chamada *Test ^ 1*, digite:

```
set testVar=test^^1
```

O comando **set** atribui tudo o que segue o sinal de igual (=) para o valor da variável. Portanto, se você digitar `set testVar=test^1` , obterá o seguinte resultado, `testVar=test^1` .

Para definir uma variável de ambiente chamada *TEST&1*, digite:

```
set testVar=test^&1
```

Para definir uma variável de ambiente denominada *include* para que a cadeia de caracteres *c:\Directory* esteja associada a ela, digite:

```
set include=c:\directory
```

Em seguida, você pode usar a cadeia de caracteres *c:\Directory* em arquivos em lotes ao colocar o nome incluir com sinais de porcentagem ( **%** ). Por exemplo, você pode usar `dir %include%` em um arquivo em lotes para exibir o conteúdo do diretório associado à variável de ambiente INCLUDE. Depois que esse comando é processado, a cadeia de caracteres c:\Directory substitui **% include%**.

Para usar o comando **set** em um programa em lotes para adicionar um novo diretório à variável de ambiente *Path* , digite:

```
@echo off
rem ADDPATH.BAT adds a new directory
rem to the path environment variable.
set path=%1;%path%
set
```

Para exibir uma lista de todas as variáveis de ambiente que começam com a letra *P*, digite:

```
set p
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)