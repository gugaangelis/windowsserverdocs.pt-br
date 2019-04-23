---
title: choice
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78d2816bff754ef04558cf37eaada7c7fafba823
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841547"
---
# <a name="choice"></a>choice



Solicita que o usuário selecionar um item em uma lista de opções de único caractere em um arquivo em lotes e, em seguida, retorna o índice da opção selecionada. Se usado sem parâmetros, **choice** exibe as opções padrão **Y** e **N**.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
choice [/c [<Choice1><Choice2><…>]] [/n] [/cs] [/t <Timeout> /d <Choice>] [/m <"Text">]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/c \<Choice1 ><Choice2><... >|Especifica a lista de opções a serem criados. As opções válidas incluem a – z, A-Z, 0-9 e caracteres ASCII estendidos (254 128). A lista padrão é "YN", que é exibido como `[Y,N]?`.|
|/n|Oculta a lista de opções, embora as opções ainda estão habilitadas e o texto da mensagem (se especificado pelo **/m**) ainda é exibido.|
|/cs|Especifica que as escolhas são diferencia maiusculas de minúsculas. Por padrão, as opções não diferenciam maiusculas de minúsculas.|
|/t \<Timeout>|Especifica o número de segundos para pausar antes de usar a opção padrão especificada por **/d**. Os valores aceitáveis são de **0** à **9999**. Se **/t** é definido como **0**, **escolha** não fará uma pausa antes de retornar a opção padrão.|
|/d \<Choice>|Especifica a opção padrão para usar após aguardar o número de segundos especificado por **/t**. A opção padrão deve ser na lista de opções especificadas em **/c**.|
|/m <"Text">|Especifica uma mensagem a ser exibida antes da lista de opções. Se **/m** não for especificado, somente a opção é exibido.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   A variável de ambiente ERRORLEVEL é definida para o índice da chave que o usuário seleciona na lista de opções. A primeira opção na lista retorna um valor de 1, a segunda um valor de 2 e assim por diante. Se o usuário pressiona uma tecla que não é uma opção válida, **opção** emitirá um aviso sonoro. Se **opção** detecta uma condição de erro, ele retorna um valor ERRORLEVEL de 255. Se o usuário pressiona CTRL + BREAK ou CTRL + C **opção** retorna um valor ERRORLEVEL igual a 0.

> [!NOTE]
> Quando você usa os valores ERRORLEVEL em um arquivo em lotes, liste-as em ordem decrescente.

## <a name="BKMK_examples"></a>Exemplos

Para apresentar as opções de Y, N e C, digite a seguinte linha em um arquivo em lotes:
```
choice /c ync
```
O seguinte aviso é exibido quando o arquivo em lotes é executado o **opção** comando:
```
[Y,N,C]?
```
Para ocultar as opções de Y, N e C, mas exibe o texto "Sim, não, ou continuar", digite a seguinte linha em um arquivo em lotes:
```
choice /c ync /n /m "Yes, No, or Continue?"
```
O seguinte aviso é exibido quando o arquivo em lotes é executado o **opção** comando:
```
Yes, No, or Continue?
```

> [!NOTE]
> Se você usar o **/n** parâmetro, mas não use **/m**, o usuário não é solicitado quando **escolha** está aguardando entrada.

Para mostrar o texto e as opções usadas nos exemplos anteriores, digite a seguinte linha em um arquivo em lotes:
```
choice /c ync /m "Yes, No, or Continue"
```
O seguinte aviso é exibido quando o arquivo em lotes é executado o **opção** comando:
```
Yes, No, or Continue [Y,N,C]?
```
Para definir um limite de tempo de cinco segundos e especificar **N** como o valor padrão, digite a seguinte linha em um arquivo em lotes:
```
choice /c ync /t 5 /d n
```
O seguinte aviso é exibido quando o arquivo em lotes é executado o **opção** comando:
```
[Y,N,C]?
```

> [!NOTE]
> Neste exemplo, se o usuário não pressionar uma tecla dentro de cinco segundos, **choice** seleciona **N** por padrão e retorna um valor de erro de 2. Caso contrário, **opção** retorna o valor correspondente para a escolha do usuário.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)