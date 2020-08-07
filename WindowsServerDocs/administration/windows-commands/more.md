---
title: mais
description: Artigo de referência para o comando more, que exibe uma tela de saída de cada vez.
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/26/2019
ms.openlocfilehash: 198f3f3f3b80282d876e4fdda9e7cde649a8c7da
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886370"
---
# <a name="more"></a>mais

Exibe uma tela de saída de cada vez.

> [!NOTE]
> O comando **more** , com parâmetros diferentes, também está disponível no console de recuperação.

## <a name="syntax"></a>Sintaxe

```
<command> | more [/c] [/p] [/s] [/t<n>] [+<n>]
more [[/c] [/p] [/s] [/t<n>] [+<n>]] < [<drive>:][<path>]<filename>
more [/c] [/p] [/s] [/t<n>] [+<n>] [<files>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<command>` | Especifica um comando para o qual você deseja exibir a saída. |
| /c | Limpa a tela antes de exibir uma página. |
| /p | Expande os caracteres de feed de formulário. |
| /s | Exibe várias linhas em branco como uma única linha em branco. |
| /t`<n>` | Exibe as guias como o número de espaços especificado por *n*. |
| +`<n>` | Exibe o primeiro arquivo, começando na linha especificada por *n*. |
| `[<drive>:][<path>]<filename>` | Especifica o local e o nome de um arquivo a ser exibido. |
| `<files>` | Especifica uma lista de arquivos a serem exibidos. Os arquivos devem ser separados usando espaços. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os seguintes subcomandos são aceitos no prompt **mais** ( `-- More --` ), incluindo:

    | Chave | Ação |
    | --- | ------ |
    | BARRA DE ESPAÇOS | Pressione a **barra de espaços** para exibir a próxima tela. |
    | Enter | Pressione **Enter** para exibir o arquivo uma linha por vez. |
    | f | Pressione **F** para exibir o próximo arquivo listado na linha de comando. |
    | q | Pressione **Q** para fechar o comando **more** . |
    | = | Mostra o número da linha. |
    | DTI`<n>` | Pressione **P** para exibir as próximas *n* linhas. |
    | &`<n>` | Pressione **S** para ignorar as próximas *n* linhas. |
    | ? | Pressionar **?** para mostrar os comandos que estão disponíveis no prompt **mais** .|

- Se você usar o caractere de redirecionamento ( `<` ), também deverá especificar um nome de arquivo como a origem.

- Se você usar o pipe ( `|` ), poderá usar comandos como **dir**, **Sort**e **Type**.

### <a name="examples"></a>Exemplos

Para exibir a primeira tela de informações de um arquivo chamado *clients. New*, digite um dos seguintes comandos:

```
more < clients.new
type clients.new | more
```

O comando **more** exibe a primeira tela de informações de clients. New, e você pode pressionar a barra de espaços para ver a próxima tela de informações.

Para limpar a tela e remover todas as linhas em branco extras antes de exibir o arquivo *clients. New*, digite um dos seguintes comandos:

```
more /c /s < clients.new
type clients.new | more /c /s
```

Para exibir o número da linha atual no prompt **mais** , digite:

```
more =
```

O número da linha atual é adicionado ao prompt **mais** , como`-- More [Line: 24] --`

Para exibir um número específico de linhas no prompt **mais** , digite:

```
more p
```

A solicitação **mais** solicita o número de linhas a serem exibidas, da seguinte maneira: `-- More -- Lines:` . Digite o número de linhas a serem exibidas e pressione ENTER. A tela é alterada para mostrar apenas o número de linhas.

Para ignorar um número específico de linhas no prompt **mais** , digite:

```
more s
```

A solicitação **mais** solicita o número de linhas a serem ignoradas, da seguinte maneira: `-- More -- Lines:` . Digite o número de linhas a serem ignoradas e pressione ENTER. A tela é alterada para mostrar que essas linhas são ignoradas.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Ambiente de recuperação do Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
