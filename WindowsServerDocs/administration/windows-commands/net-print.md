---
title: net print
description: Artigo de referência do comando net Print. Este comando foi preterido e não tem garantia de suporte em versões futuras do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ac91d2226e9a5394d6f7ea00ab6f268eb99015b
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956798"
---
# <a name="net-print"></a>net print

> [!IMPORTANT]
> Este comando foi preterido. No entanto, você pode executar muitas das mesmas tarefas usando o [comando prnjobs](prnjobs.md), [Instrumentação de gerenciamento do Windows (WMI)](/windows/win32/wmisdk/wmi-start-page), [Gerenciamento de imgestão no PowerShell](/powershell/module/printmanagement)ou [recursos de script para profissionais de ti](https://gallery.technet.microsoft.com/ScriptCenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=printing&f%5B0%5D.Text=Printing).

Exibe informações sobre uma fila de impressora especificada ou um trabalho de impressão especificado, ou controla um trabalho de impressão especificado.

## <a name="syntax"></a>Sintaxe

```
net print {\\<computername>\<sharename> | \\<computername> <jobnumber> [/hold | /release | /delete]} [help]
```

### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
| ---------- | ----------- |
| `\\<computername>\<sharename>` | Especifica (por nome) o computador e a fila de impressão sobre os quais você deseja exibir informações. |
| `\\<computername>` | Especifica (por nome) o computador que hospeda o trabalho de impressão que você deseja controlar. Se você não especificar um computador, o computador local será assumido. Requer o `<jobnumber>` parâmetro. |
| `<jobnumber>` | Especifica o número do trabalho de impressão que você deseja controlar. Esse número é atribuído pelo computador que hospeda a fila de impressão onde o trabalho de impressão é enviado. Depois que um computador atribui um número a um trabalho de impressão, esse número não é atribuído a outros trabalhos de impressão em nenhuma fila hospedada por esse computador. Necessário ao usar o `\\<computername>` parâmetro. |
| `[/hold | /release | /delete]` | Especifica a ação a ser tomada com o trabalho de impressão. Se você especificar um número de trabalho, mas não especificar nenhuma ação, as informações sobre o trabalho de impressão serão exibidas.<ul><li>**/Hold** -atrasa o trabalho, permitindo que outros trabalhos de impressão o ignorem até que sejam liberados.</li><li>**/Release** -libera um trabalho de impressão que foi atrasado.</li><li>**/delete** – remove um trabalho de impressão de uma fila de impressão.</li></ul> |
| ajuda | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- O `net print\\<computername>` comando exibe informações sobre trabalhos de impressão em uma fila de impressora compartilhada. Veja a seguir um exemplo de um relatório para todos os trabalhos de impressão em uma fila para uma impressora compartilhada chamada *laser*:

    ```
    printers at \\PRODUCTION
    Name              Job #      Size      Status
    -----------------------------
    LASER Queue       3 jobs               *printer active*
    USER1          84        93844      printing
    USER2          85        12555      Waiting
    USER3          86        10222      Waiting
    ```

- Veja a seguir um exemplo de um relatório para um trabalho de impressão:

    ```
    Job #            35
    Status           Waiting
    Size             3096
    remark
    Submitting user  USER2
    Notify           USER2
    Job data type
    Job parameters
    additional info
    ```

### <a name="examples"></a>Exemplos

Para listar o conteúdo da fila de impressão *Dotmatrix* no computador de * \\ produção* , digite:

```
net print \\Production\Dotmatrix
```

Para exibir informações sobre o número de trabalho *35* no computador de * \\ produção* , digite:

```
net print \\Production 35
```

Para atrasar o número de trabalho *263* no computador de * \\ produção* , digite:

```
net print \\Production 263 /hold
```

Para liberar o número de trabalho *263* no computador de * \\ produção* , digite:

```
net print \\Production 263 /release
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [referência de comando de impressão](print-command-reference.md)

- [comando prnjobs](prnjobs.md)

- [WMI (Instrumentação de Gerenciamento do Windows)](/windows/win32/wmisdk/wmi-start-page)

- [Gerenciamento de imgestão no PowerShell](/powershell/module/printmanagement)

- [Recursos de script para profissionais de ti](https://gallery.technet.microsoft.com/ScriptCenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=printing&f%5B0%5D.Text=Printing)
