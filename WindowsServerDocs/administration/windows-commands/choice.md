---
title: opção
description: Artigo de referência para o comando Choice, que solicita ao usuário que selecione um item de uma lista de opções de caractere único em um programa em lotes e, em seguida, retorna o índice da opção selecionada.
ms.topic: reference
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 382e3618e66f56e05ebd0a7d6b6034e6d7543d64
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629683"
---
# <a name="choice"></a>opção

Solicita que o usuário selecione um item de uma lista de opções de caractere único em um programa em lotes e, em seguida, retorne o índice da opção selecionada. Se usado sem parâmetros, **Choice** exibirá as opções padrão **Y** e **N**.

## <a name="syntax"></a>Sintaxe

```
choice [/c [<choice1><choice2><…>]] [/n] [/cs] [/t <timeout> /d <choice>] [/m <text>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /c `<choice1><choice2><…>` | Especifica a lista de opções a serem criadas. As opções válidas incluem a-z, A-Z, 0-9 e caracteres ASCII estendidos (128-254). A lista padrão é YN, que é exibida como `[Y,N]?` . |
| /n | Oculta a lista de opções, embora as opções ainda estejam habilitadas e o texto da mensagem (se especificado por **/m**) ainda seja exibido. |
| /cs | Especifica que as opções diferenciam maiúsculas de minúsculas. Por padrão, as opções não diferenciam maiúsculas de minúsculas. |
| /t `<timeout>` | Especifica o número de segundos para pausar antes de usar a opção padrão especificada por **/d**. Os valores aceitáveis são de **0** a **9999**. Se **/t** for definido como **0**, a **opção** não será pausada antes de retornar a escolha padrão. |
| /d `<choice>` | Especifica a escolha padrão a ser usada depois de aguardar o número de segundos especificado por **/t**. A opção padrão deve estar na lista de opções especificadas por **/c**. |
| opção `<text>` | Especifica uma mensagem a ser exibida antes da lista de opções. Se **/m** não for especificado, somente o prompt de opção será exibido. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários

- A variável de ambiente **ERRORLEVEL** é definida como o índice da chave que o usuário seleciona na lista de opções. A primeira opção na lista retorna um valor de `1` , o segundo valor de `2` e assim por diante. Se o usuário pressionar uma tecla que não seja uma opção válida, **Choice** emitirá um aviso sonoro.

- Se **Choice** detectar uma condição de erro, retornará um valor **ERRORLEVEL** de `255` . Se o usuário pressionar CTRL + BREAK ou CTRL + C, **Choice** retornará um valor **ERRORLEVEL** de `0` .

> [!NOTE]
> Ao usar valores **ERRORLEVEL** em um programa em lotes, você deve listá-los em ordem decrescente.

## <a name="examples"></a>Exemplos

Para apresentar as opções **Y**, **N**e **C**, digite a seguinte linha em um arquivo em lotes:

```
choice /c ync
```

O prompt a seguir é exibido quando o arquivo em lotes executa o comando **Choice** :

```
[Y,N,C]?
```

Para ocultar as opções **Y**, **N**e **C**, mas exibir o texto **Sim**, **não**ou **continuar**, digite a seguinte linha em um arquivo em lotes:

```
choice /c ync /n /m Yes, No, or Continue?
```

> [!NOTE]
> Se você usar o parâmetro **/n** , mas não usar **/m**, o usuário não receberá uma solicitação quando a **opção** estiver aguardando a entrada.

Para mostrar o texto e as opções usadas nos exemplos anteriores, digite a seguinte linha em um arquivo em lotes:

```
choice /c ync /m Yes, No, or Continue
```

Para definir um limite de tempo de cinco segundos e especificar **N** como o valor padrão, digite a seguinte linha em um arquivo em lotes:

```
choice /c ync /t 5 /d n
```

> [!NOTE]
> Neste exemplo, se o usuário não pressionar uma tecla em cinco segundos, **Choice** selecionará **N** por padrão e retornará um valor de erro de `2` . Caso contrário, **Choice** retornará o valor correspondente à opção do usuário.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
