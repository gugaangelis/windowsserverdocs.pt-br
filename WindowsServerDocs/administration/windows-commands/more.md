---
title: mais
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/26/2019
ms.openlocfilehash: 4c627e003e71cb2265c717669e082d48564dd483
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839419"
---
# <a name="more"></a>mais



Exibe uma tela de saída de cada vez.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
<Command> | more [/c] [/p] [/s] [/t<N>] [+<N>]
more [[/c] [/p] [/s] [/t<N>] [+<N>]] < [<Drive>:][<Path>]<FileName>
more [/c] [/p] [/s] [/t<N>] [+<N>] [<Files>]
```

### <a name="parameters"></a>Parâmetros

|           Parâmetro            |                               Descrição                               |
|--------------------------------|-------------------------------------------------------------------------|
|           > de comando \<           |      Especifica um comando para o qual você deseja exibir a saída.      |
|               /c               |               Limpa a tela antes de exibir uma página.               |
|               /p               |                      Expande os caracteres de feed de formulário.                      |
|               /s               |          Exibe várias linhas em branco como uma única linha em branco.          |
|             /t\<N >             |         Exibe as guias como o número de espaços especificado por *N*.         |
|             +\<N >              |     Exibe o primeiro arquivo que começa na linha especificada por *N*.     |
| [\<drive >:] [\<caminho >]\<nome do arquivo > |          Especifica o local e o nome de um arquivo a ser exibido.          |
|            Arquivos de \<>            | Especifica uma lista de arquivos a serem exibidos. Separe os nomes de arquivos com um espaço. |
|               /?               |                  Exibe a ajuda no prompt de comando.                   |

## <a name="remarks"></a>Comentários

-   Os seguintes subcomandos são aceitos no prompt **mais** (`-- More --`). 

    | Chave | Ação |
    | --- | ------ |
    | PRESSIONA | Exibe a próxima página. |
    | ENTER | Exibe a próxima linha. |
    | f | Exibe o próximo arquivo. |
    | q | Encerra o comando **more** . |
    | = | Mostra o número da linha. |
    | p \<N > | Exibe as próximas *N* linhas. |
    | s \<N > |S KIPS as próximas *N* linhas. |
    | ? | Mostra os comandos que estão disponíveis no prompt **mais** .| 
    
-   Ao usar o caractere de redirecionamento ( **<** ), você deve especificar um nome de arquivo como a origem. Ao usar o pipe ( **\|** ), você pode usar comandos como **dir**, **Sort**e **Type**.
-   O comando **more** , com parâmetros diferentes, está disponível no console de recuperação.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para exibir a primeira tela de informações de um arquivo chamado clients. New, digite um dos seguintes comandos:
```
more < clients.new
type clients.new | more
```
O comando **more** exibe a primeira tela de informações de clients. New e, em seguida, exibe o seguinte prompt:
```
-- More --
```
Em seguida, você pode pressionar a barra de espaços para ver a próxima tela de informações.

Para limpar a tela e remover todas as linhas em branco extras antes de exibir o arquivo clients. New, digite um dos seguintes comandos:
```
more /c /s < clients.new
type clients.new | more /c /s
```
O comando **more** exibe a primeira tela de informações de clients. New e, em seguida, exibe o seguinte prompt:
```
-- More --
```

### <a name="using-more-subcommands"></a>Usando mais subcomandos

Os exemplos a seguir podem ser usados no prompt **mais** (`-- More --`).
- Para exibir o arquivo uma linha por vez, pressione ENTER no prompt **mais** .
- Para exibir a próxima tela, pressione a barra de espaços no prompt **mais** .
- Para exibir o próximo arquivo listado na linha de comando, digite **f** no prompt **mais** .
- Para mostrar os comandos disponíveis, digite **?** no prompt **mais** .
- Para encerrar **mais**, digite **q** no prompt **mais** .
- Para exibir o número de linha atual, digite **=** no prompt **mais** . O número da linha atual é adicionado ao prompt **mais** da seguinte maneira:  
  ```
  -- More [Line: 24] --
  ```  
- Para exibir um número específico de linhas, digite **p** no prompt **mais** . **Mais** solicita o número de linhas a serem exibidas da seguinte maneira:  
  ```
  -- More -- Lines:
  ```  
  Digite o número de linhas a serem exibidas e pressione ENTER. **Mais** exibe o número especificado de linhas.
- Para ignorar um número específico de linhas, digite **s** no prompt **mais** . **Mais** solicita o número de linhas a serem ignoradas da seguinte maneira:  
  ```
  -- More -- Lines:
  ```  
  Digite o número de linhas a serem ignoradas e pressione ENTER. **Mais** ignora o número especificado de linhas e exibe a próxima tela de informações.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
