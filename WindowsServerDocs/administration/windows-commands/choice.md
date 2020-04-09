---
title: escolha
description: Tópico de comandos do Windows para escolher, que solicita ao usuário que selecione um item de uma lista de opções de caractere único em um programa em lotes e, em seguida, retorna o índice da opção selecionada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26f915bc35b9edf3b206ff65e011b7f4a22988b7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847790"
---
# <a name="choice"></a>escolha

Solicita que o usuário selecione um item de uma lista de opções de caractere único em um programa em lotes e, em seguida, retorne o índice da opção selecionada. Se usado sem parâmetros, **Choice** exibirá as opções padrão **Y** e **N**.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
choice [/c [<Choice1><Choice2><…>]] [/n] [/cs] [/t <Timeout> /d <Choice>] [/m <Text>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/c \<Choice1 ><Choice2><... >|Especifica a lista de opções a serem criadas. As opções válidas incluem a-z, A-Z, 0-9 e caracteres ASCII estendidos (128-254). A lista padrão é YN, que é exibida como `[Y,N]?`.|
|/n|Oculta a lista de opções, embora as opções ainda estejam habilitadas e o texto da mensagem (se especificado por **/m**) ainda seja exibido.|
|/cs|Especifica que as opções diferenciam maiúsculas de minúsculas. Por padrão, as opções não diferenciam maiúsculas de minúsculas.|
|/t \<tempo limite >|Especifica o número de segundos para pausar antes de usar a opção padrão especificada por **/d**. Os valores aceitáveis são de **0** a **9999**. Se **/t** for definido como **0**, a **opção** não será pausada antes de retornar a escolha padrão.|
|/d \<opção >|Especifica a escolha padrão a ser usada depois de aguardar o número de segundos especificado por **/t**. A opção padrão deve estar na lista de opções especificadas por **/c**.|
|/m <Text>|Especifica uma mensagem a ser exibida antes da lista de opções. Se **/m** não for especificado, somente o prompt de opção será exibido.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   A variável de ambiente ERRORLEVEL é definida como o índice da chave que o usuário seleciona na lista de opções. A primeira opção na lista retorna um valor de 1, o segundo valor de 2 e assim por diante. Se o usuário pressionar uma tecla que não seja uma opção válida, **Choice** emitirá um aviso sonoro. Se **Choice** detectar uma condição de erro, retornará um valor ERRORLEVEL de 255. Se o usuário pressionar CTRL + BREAK ou CTRL + C, **Choice** retornará um valor ERRORLEVEL de 0.

> [!NOTE]
> Quando você usa valores ERRORLEVEL em um programa em lotes, listá-los em ordem decrescente.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para apresentar as opções Y, N e C, digite a seguinte linha em um arquivo em lotes:
```
choice /c ync
```
O prompt a seguir é exibido quando o arquivo em lotes executa o comando **Choice** :
```
[Y,N,C]?
```
Para ocultar as opções Y, N e C, mas exibir o texto Sim, não ou continuar, digite a seguinte linha em um arquivo em lotes:
```
choice /c ync /n /m Yes, No, or Continue?
```
O prompt a seguir é exibido quando o arquivo em lotes executa o comando **Choice** :
```
Yes, No, or Continue?
```

> [!NOTE]
> Se você usar o parâmetro **/n** , mas não usar **/m**, o usuário não receberá uma solicitação quando a **opção** estiver aguardando a entrada.

Para mostrar o texto e as opções usadas nos exemplos anteriores, digite a seguinte linha em um arquivo em lotes:
```
choice /c ync /m Yes, No, or Continue
```
O prompt a seguir é exibido quando o arquivo em lotes executa o comando **Choice** :
```
Yes, No, or Continue [Y,N,C]?
```
Para definir um limite de tempo de cinco segundos e especificar **N** como o valor padrão, digite a seguinte linha em um arquivo em lotes:
```
choice /c ync /t 5 /d n
```
O prompt a seguir é exibido quando o arquivo em lotes executa o comando **Choice** :
```
[Y,N,C]?
```

> [!NOTE]
> Neste exemplo, se o usuário não pressionar uma tecla em cinco segundos, **Choice** selecionará **N** por padrão e retornará um valor de erro 2. Caso contrário, **Choice** retornará o valor correspondente à opção do usuário.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)