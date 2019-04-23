---
title: more
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4957f9455c6caed027331a939db0c2fefbe1961
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857567"
---
# <a name="more"></a>more



Exibe uma tela de saída por vez.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
<Command> | more [/c] [/p] [/s] [/t<N>] [+<N>]
more [[/c] [/p] [/s] [/t<N>] [+<N>]] < [<Drive>:][<Path>]<FileName>
more [/c] [/p] [/s] [/t<N>] [+<N>] [<Files>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Comando >|Especifica um comando para o qual você deseja exibir a saída.|
|/c|Limpa a tela antes de exibir uma página.|
|/p|Expande os caracteres de avanço.|
|/s|Exibe várias linhas em branco como uma única linha em branco.|
|/t\<N>|Exibe as guias como o número de espaços especificado por *N*.|
|+\<N>|Exibe o primeiro arquivo começando na linha especificada por *N*.|
|[\<Drive>:] [<Path>]<FileName>|Especifica o local e o nome do arquivo a ser exibido.|
|\<Arquivos >|Especifica uma lista de arquivos a serem exibidos. Separe os nomes de arquivo com um espaço.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Os subcomandos a seguir são aceitas na **mais** prompt (`-- More --`).  
    |Chave|Ação|
    |---|------|
    |BARRA DE ESPAÇOS|Exibe a próxima página.|
    |ENTER|Exibe a próxima linha.|
    |f|Exibe o próximo arquivo.|
    |q|Encerra o **mais** comando.|
    |=|Mostra o número de linha.|
    |p \<N>|Exibe a próxima *N* linhas.|
    |s \<N>|Ignora a próxima *N* linhas.|
    |?|Mostra os comandos que estão disponíveis na **mais** prompt.|
-Ao usar o caractere de redirecionamento (**<**), você deve especificar um nome de arquivo como a origem. Ao usar o pipe (* *|*), você pode usar esses comandos como **dir**, **classificação**, e **tipo**.
-   O **mais** comando com parâmetros diferentes, está disponível no Console de recuperação.

## <a name="BKMK_examples"></a>Exemplos

Para exibir a primeira tela de informações de um arquivo chamado Clients, digite um dos seguintes comandos:
```
more < clients.new
type clients.new | more
```
O **mais** comando exibe a primeira tela de informações de Nov e, em seguida, exibe o prompt a seguir:
```
-- More --
```
Em seguida, você pode pressionar a barra de espaços para ver a próxima tela de informações.

Para limpar a tela e remover todas as linhas em branco extra antes de exibir o arquivo Clients, digite um dos seguintes comandos:
```
more /c /s < clients.new
type clients.new | more /c /s
```
O **mais** comando exibe a primeira tela de informações de Nov e, em seguida, exibe o prompt a seguir:
```
-- More --
```

### <a name="using-more-subcommands"></a>Usando subcomandos mais

Os exemplos a seguir podem ser usados com o **mais** prompt (`-- More --`).
-   Para exibir a arquivo uma linha por vez, pressione ENTER na **mais** prompt.
-   Para exibir a próxima tela, pressione a barra de espaços na **mais** prompt.
-   Para exibir o próximo arquivo listado na linha de comando, digite **f** com o **mais** prompt.
-   Para mostrar os comandos disponíveis, digite **?** com o **mais** prompt.
-   Para encerrar **mais**, tipo **q** no **mais** prompt.
-   Para exibir o número da linha atual, digite **=** com o **mais** prompt. O número da linha atual é adicionado para o **mais** prompt da seguinte maneira:  
    ```
    -- More [Line: 24] --
    ```  
-   Para exibir um número específico de linhas, digite **p** com o **mais** prompt. **Mais** solicitará que o número de linhas a serem exibidas da seguinte maneira:  
    ```
    -- More -- Lines:
    ```  
    Digite o número de linhas a serem exibidas e, em seguida, pressione ENTER. **Mais** exibe o número de linhas especificado.
-   Para ignorar um número específico de linhas, digite **s** com o **mais** prompt. **Mais** solicitará que o número de linhas a serem ignoradas da seguinte maneira:  
    ```
    -- More -- Lines:
    ```  
    Digite o número de linhas a serem ignoradas e, em seguida, pressione ENTER. **Mais** ignora o número de linhas especificado e exibe a próxima tela de informações.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)