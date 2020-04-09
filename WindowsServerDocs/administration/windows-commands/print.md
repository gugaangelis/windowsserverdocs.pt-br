---
title: print
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa2325d5-a993-4ed3-b996-255165452db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36966d8d3beb032ee0dcee50d9bd5bc0111bf4f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837369"
---
# <a name="print"></a>print



Envia um arquivo de texto para uma impressora.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Print [/d:<PrinterName>] [<Drive>:][<Path>]<FileName>[ ...]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/d:\<PrinterName >|Especifica a impressora na qual você deseja imprimir o trabalho. Para imprimir em uma impressora conectada localmente, especifique a porta no computador em que a impressora está conectada.</br>-Os valores válidos para as portas paralelas são LPT1, LPT2 e LPT3.</br>-Os valores válidos para as portas seriais são COM1, COM2, COM3 e COM4.</br>Você também pode especificar uma impressora de rede usando seu nome de fila (\\\\*servername*\*impressoraname *). Se você não especificar uma impressora, o trabalho de impressão será enviado para LPT1 por padrão.|
|> da unidade de \<:|Especifica a unidade lógica ou física na qual o arquivo que você deseja imprimir está localizado. Esse parâmetro não será necessário se o arquivo que você deseja imprimir estiver localizado na unidade atual.|
|\<caminho >|Especifica o local do arquivo que você deseja imprimir. Esse parâmetro não será necessário se o arquivo que você deseja imprimir estiver localizado no diretório atual.|
|\<nome de arquivo > [...]|Obrigatório. Especifica o arquivo que você deseja imprimir. Você pode incluir vários arquivos em um comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Um arquivo pode ser impresso em segundo plano se você o enviar para uma impressora conectada a uma porta serial ou paralela no computador local.
-   Você pode executar muitas tarefas de configuração no prompt de comando usando o comando **Mode** .

    Consulte o [modo](mode.md) para obter mais informações sobre:  
    -   Configurando uma impressora conectada a uma porta paralela
    -   Configurando uma impressora conectada a uma porta serial
    -   Exibindo o status de uma impressora
    -   Preparando uma impressora para alternância de página de código

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para enviar o arquivo Report. txt no diretório atual para uma impressora conectada ao LPT2 no computador local, digite:
```
print /d:lpt2 report.txt
```
Para enviar o arquivo Report. txt no diretório c:\Accounting para a fila de impressão Printer1 no servidor \\\\CopyRoom, digite:
```
print /d:\\copyroom\printer1 c:\accounting\report.txt 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Referência aos comandos de impressão](print-command-reference.md)

[Modo](mode.md)