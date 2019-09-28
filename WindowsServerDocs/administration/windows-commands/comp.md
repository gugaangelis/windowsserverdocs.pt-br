---
title: comp
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40319d23-704d-4da1-be93-8259547275d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84604cea36b0b4c9543a7169002551c0da4f0493
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379262"
---
# <a name="comp"></a>comp



Compara o conteúdo de dois arquivos ou conjuntos de arquivos byte a byte. Se usado sem parâmetros, **comp** solicitará que você insira os arquivos a serem comparados.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
comp [<Data1>] [<Data2>] [/d] [/a] [/l] [/n=<Number>] [/c]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Data1 >|Especifica o local e o nome do primeiro arquivo ou conjunto de arquivos que você deseja comparar. Você pode usar caracteres curinga ( **&#42;** e **?** ) para especificar vários arquivos.|
|\<Data2 >|Especifica o local e o nome do segundo arquivo ou conjunto de arquivos que você deseja comparar. Você pode usar caracteres curinga ( **&#42;** e **?** ) para especificar vários arquivos.|
|/d|Exibe as diferenças no formato decimal. (O formato padrão é hexadecimal.)|
|SRDF|Exibe as diferenças como caracteres.|
|/l|Exibe o número da linha em que ocorre uma diferença, em vez de exibir o deslocamento de byte.|
|/n = \<Number >|Compara somente o número de linhas que são especificadas para cada arquivo, mesmo que os arquivos sejam tamanhos diferentes.|
|/c|Executa uma comparação que não diferencia maiúsculas de minúsculas.|
|/off [linha]|Processa arquivos com o atributo offline definido.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Como o comando **comp** identifica informações incompatíveis

    Durante a comparação, **comp** exibe mensagens que identificam os locais de informações desiguais entre os arquivos. Cada mensagem indica o endereço de deslocamento de memória dos bytes desiguais e o conteúdo dos bytes (em notação hexadecimal, a menos que o parâmetro de linha de comando **/a** ou **/d** seja especificado). As mensagens aparecem no seguinte formato:

    `Compare error at OFFSET xxxxxxxx`

    `file1 = xx`

    `file2 = xx`

    Depois de dez comparações desiguais, o **comp** para de comparar os arquivos e exibe a seguinte mensagem:

    `10 Mismatches - ending compare`
-   Lidando com casos especiais de *dados1* e *data2*  
    -   Se você omitir os componentes necessários de *dados1* ou *data2* ou se omitir *data2*, o **comp** solicitará as informações ausentes.
    -   Se *dados1* contiver apenas uma letra de unidade ou um nome de diretório sem nome de arquivo, **comp** comparará todos os arquivos no diretório especificado para o arquivo especificado em *data1*.
    -   Se *data2* contiver apenas uma letra de unidade ou um nome de diretório, o nome de arquivo padrão para *data2* será o mesmo que em *dados1*.
    -   Se **comp** não conseguir localizar os arquivos que você especificar, ele solicitará uma mensagem para determinar se você deseja comparar mais arquivos.
-   Comparando arquivos em locais diferentes

    O **comp** pode comparar arquivos na mesma unidade ou em unidades diferentes, e no mesmo diretório ou em diretórios diferentes. Quando a **comp** compara os arquivos, ele exibe seus locais e nomes de arquivo.
-   Comparando arquivos com os mesmos nomes

    Os arquivos que você compara podem ter o mesmo nome de arquivo, desde que estejam em diretórios diferentes ou em unidades diferentes. Se você não especificar um nome de arquivo para *data2*, o nome de arquivo padrão para *data2* será o mesmo que o nome do arquivo em *dados1*. Você pode usar caracteres curinga ( **&#42;** e **?** ) para especificar nomes de arquivo.
-   Comparando arquivos de tamanhos diferentes

    Você deve especificar **/n** para comparar arquivos de tamanhos diferentes. Se os tamanhos de arquivo forem diferentes e **/n** não for especificado, **comp** exibirá a seguinte mensagem:

    `Files are different sizes`

    `Compare more files (Y/N)?`

    Para comparar esses arquivos, pressione N para interromper o comando **comp** . Em seguida, execute novamente o comando **comp** com a opção **/n** para comparar apenas a primeira parte de cada arquivo.
-   Comparando arquivos sequencialmente

    Se você usar caracteres curinga ( **&#42;** e **?** ) para especificar vários arquivos, o **comp** encontrará o primeiro arquivo que corresponde a *dados1* e o compara com o arquivo correspondente em *data2*, se existir. O comando **comp** relata os resultados da comparação para cada arquivo correspondente a *dados1*. Quando terminar, **comp** exibirá a seguinte mensagem:

    `Compare more files (Y/N)?`

    Para comparar mais arquivos, pressione Y. O comando **comp** solicita os locais e nomes dos novos arquivos. Para interromper as comparações, pressione N. Quando você pressionar s, o **comp** solicitará as opções de linha de comando a serem usadas. Se você não especificar nenhuma opção de linha de comando, **comp** usará aquelas que você especificou anteriormente.

## <a name="BKMK_examples"></a>Disso

Para comparar o conteúdo do diretório C:\Reports com o diretório de backup \\ @ no__t-1Sales\Backup\April, digite:
```
comp c:\reports \\sales\backup\april
```
Para comparar as dez primeiras linhas dos arquivos de texto no diretório \Invoice e exibir o resultado em formato decimal, digite:
```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)