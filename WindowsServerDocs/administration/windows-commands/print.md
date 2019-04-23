---
title: print
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d85fc5b2cd5f5ba09ebdf4756a5adb60c1759f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831547"
---
# <a name="print"></a>print



Envia um arquivo de texto para uma impressora.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Print [/d:<PrinterName>] [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/d:\<PrinterName>|Especifica a impressora que você deseja que o trabalho de impressão. Para imprimir uma impressora conectada localmente, especifique a porta no computador em que a impressora está conectada.</br>-Valores válidos para portas paralelas são LPT1, LPT2 e LPT3.</br>-Valores válidos para as portas seriais são COM1, COM2, COM3 e COM4.</br>Você também pode especificar uma impressora de rede usando seu nome de fila (\\\\*ServerName*\*NomeDaImpressora *). Se você não especificar uma impressora, o trabalho de impressão é enviado para LPT1 por padrão.|
|\<Drive>:|Especifica a unidade lógica ou física em que se encontra o arquivo que você deseja imprimir. Esse parâmetro não é necessário se o arquivo que você deseja imprimir está localizado na unidade atual.|
|\<Caminho >|Especifica o local do arquivo que você deseja imprimir. Esse parâmetro não é necessário se o arquivo que você deseja imprimir está localizado no diretório atual.|
|\<FileName>[ ...]|Obrigatório. Especifica o arquivo que você deseja imprimir. Você pode incluir vários arquivos em um comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Um arquivo pode imprimir em segundo plano se você enviá-la em uma impressora conectada a uma porta serial ou paralela no computador local.
-   Você pode executar muitas tarefas de configuração do prompt de comando usando o **modo** comando.

    Ver [modo](mode.md) para obter mais informações sobre:  
    -   Configurar uma impressora conectada a uma porta paralela
    -   Configurar uma impressora conectada a uma porta serial
    -   Exibir o status de uma impressora
    -   Preparando-se uma impressora para a alternância da página de código

## <a name="BKMK_examples"></a>Exemplos

Para enviar o arquivo Report. txt no diretório atual para uma impressora conectada a LPT2 no computador local, digite:
```
print /d:lpt2 report.txt
```
Para enviar o arquivo Report. txt no diretório c:\Accounting à fila de impressão Impres1 o \\ \\saladerep servidor, digite:
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

[Referência do comando Imprimir](print-command-reference.md)

[Modo](mode.md)