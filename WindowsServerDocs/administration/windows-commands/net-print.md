---
title: Net print
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 441a61756869442fb91d26bfacc64bbeb8b902f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826007"
---
# <a name="net-print"></a>Net print

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre uma fila de impressora especificada ou um trabalho de impressão especificado ou controla um trabalho de impressão especificado.
Para obter exemplos de como usar esse comando, consulte o [exemplos](#BKMK_examples) seção deste documento.
> [!NOTE]
> Esse comando foi preterido no Windows 7 e Windows Server 2008 R2. No entanto, você pode executar muitas das mesmas tarefas usando cmdlets do Windows PowerShell, Windows Management Instrumentation (WMI) ou prnjobs. Para obter mais informações, consulte [prnjobs](prnjobs.md), [instrumentação de gerenciamento do Windows](https://go.microsoft.com/fwlink/?LinkID=29991) (https://go.microsoft.com/fwlink/?LinkID=29991), [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=128426) (https://go.microsoft.com/fwlink/?LinkID=128426)e o [Galeria do TechNet Script Center](https://go.microsoft.com/fwlink/?LinkId=164635) (https://go.microsoft.com/fwlink/?LinkId=164635).
## <a name="syntax"></a>Sintaxe
```
Net print {\\<computerName>\<Sharename> | 
\\<computerName> <JobNumber> [/hold | /release | /delete]} [help]
```
## <a name="parameters"></a>Parâmetros
|Parâmetros|Descrição|
|-------|--------|
|\\\\<computerName>\\<Sharename>|Especifica a fila de impressão e de computador sobre o qual você deseja exibir informações (por nome).|
|\\\\<computerName>|Especifica o computador que hospeda o trabalho de impressão que você deseja controlar (por nome). Se você não especificar um computador, o computador local será assumido. Requer a <JobNumber> parâmetro.|
|<JobNumber>|Especifica o número do trabalho de impressão que você deseja controlar. Esse número é atribuído pelo computador que hospeda a fila de impressão em que o trabalho de impressão é enviado. Depois que um computador atribui um número a um trabalho de impressão que o número não está atribuído a quaisquer outros trabalhos de impressão em qualquer fila hospedada pelo computador. Necessário ao usar o \\ \\ <computerName> parâmetro.|
|[/hold &#124; /release &#124; /delete]|Especifica a ação a ser tomada com o trabalho de impressão.<br /><br />-A **/mantenha** parâmetro atrasa o trabalho, permitindo que outros trabalhos de impressão para contorná-lo até que ele seja liberado.<br />-A **/versão** parâmetro libera um trabalho de impressão foi adiado.<br />-A **/excluir** parâmetro remove um trabalho de impressão de uma fila de impressa.<br /><br />Se você especifica um número de trabalho, mas não especificar qualquer ação, informações sobre o trabalho de impressão serão exibidas.|
|ajuda|Exibe a Ajuda para o **Net print** comando.|
## <a name="remarks"></a>Comentários
-   **Net print** \\ \\ <computerName> exibe informações sobre trabalhos de impressão em uma fila de impressora compartilhada. Este é um exemplo de um relatório de todos os trabalhos de impressão em uma fila para uma impressora compartilhada chamada LASER:
    ```
    printers at \\PRODUCTION
    Name              Job #      Size      Status
    -----------------------------
    LASER Queue       3 jobs               *printer active*
       USER1          84        93844      printing
       USER2          85        12555      Waiting
       USER3          86        10222      Waiting
    ```
-   Este é um exemplo de um relatório para um trabalho de impressão:
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
## <a name="BKMK_examples"></a>Exemplos
Este exemplo mostra como listar o conteúdo da fila de impressão Dotmatrix no \\\Production computador:
```
Net print \\Production\Dotmatrix 
```
Este exemplo mostra como exibir informações sobre o trabalho de número 35 sobre o \\\Production computador:
```
Net print \\Production 35 
```
Este exemplo mostra como atrasar o trabalho de número 263 o \\\Production computador:
```
Net print \\Production 263 /hold 
```
Este exemplo mostra como liberar o trabalho de número 263 no \\\Production computador:
```
Net print \\Production 263 /release 
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência do comando Imprimir](print-command-reference.md)
