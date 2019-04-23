---
title: comp
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d10b86176d97e1afd76085516fbfc00bdc36577f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854677"
---
# <a name="comp"></a>comp



Compara o conteúdo de dois arquivos ou grupos de arquivos byte por byte. Se usado sem parâmetros, **comp** solicita que você insira os arquivos a ser comparado.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
comp [<Data1>] [<Data2>] [/d] [/a] [/l] [/n=<Number>] [/c]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Data1 >|Especifica o local e o nome do primeiro arquivo ou conjunto de arquivos que você deseja comparar. Você pode usar caracteres curinga (**&#42;** e **?**) para especificar vários arquivos.|
|\<Data2 >|Especifica o local e o nome do segundo arquivo ou conjunto de arquivos que você deseja comparar. Você pode usar caracteres curinga (**&#42;** e **?**) para especificar vários arquivos.|
|/d|Exibe as diferenças no formato decimal. (O formato padrão é hexadecimal).|
|/a|Exibe as diferenças como caracteres.|
|/l|Exibe o número da linha onde ocorre uma diferença, em vez de exibir o deslocamento de bytes.|
|/n=\<Number>|Compara apenas o número de linhas que são especificados para cada arquivo, mesmo se os arquivos são de tamanhos diferentes.|
|/c|Executa uma comparação que não diferencia maiusculas de minúsculas.|
|/off[line]|Processa arquivos com o conjunto de atributos off-line.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Como o **comp** comando identifica informações incompatível

    Durante a comparação **comp** exibe mensagens que identificam os locais de informações diferentes entre os arquivos. Cada mensagem indica que o endereço de memória de deslocamento de bytes desiguais e o conteúdo dos bytes (em notação hexadecimal, a menos que o **/a** ou **/d** parâmetro de linha de comando for especificado). Mensagens são exibidas no seguinte formato:

    `Compare error at OFFSET xxxxxxxx`

    `file1 = xx`

    `file2 = xx`

    Após dez comparações desiguais **comp** interrompe a comparação dos arquivos e exibe a seguinte mensagem:

    `10 Mismatches - ending compare`
-   Lidar com casos especiais para *Data1* e *Data2*  
    -   Se você omitir os componentes necessários do *Data1* ou *Data2* ou se você omitir *Data2*, **comp** solicitará as informações ausentes.
    -   Se *Data1* contém apenas uma letra de unidade ou um nome de diretório e nenhum nome de arquivo **comp** compara todos os arquivos no diretório especificado para o arquivo especificado no *Data1*.
    -   Se *Data2* contém apenas uma letra de unidade ou um nome de diretório, o nome de arquivo padrão para *Data2* é o mesmo que no *Data1*.
    -   Se **comp** não é possível localizar os arquivos especificados, ele emitirá uma mensagem para determinar se você deseja comparar mais arquivos.
-   Comparando arquivos em locais diferentes

    **Comp** pode comparar arquivos na mesma unidade ou em unidades diferentes e no mesmo diretório ou em diretórios diferentes. Quando **comp** compara os arquivos, ele exibe seus locais e nomes de arquivo.
-   Comparando arquivos com os mesmos nomes

    Os arquivos que você compare podem ter o mesmo nome de arquivo, desde que eles estejam em diretórios diferentes ou em unidades diferentes. Se você não especificar um nome de arquivo para *Data2*, o nome de arquivo padrão para *Data2* é o mesmo que o nome do arquivo *Data1*. Você pode usar caracteres curinga (**&#42;** e **?**) para especificar nomes de arquivo.
-   Comparando arquivos de tamanhos diferentes

    Você deve especificar **/n** para comparar arquivos de tamanhos diferentes. Se os tamanhos dos arquivos forem diferentes e **/n** não for especificado, **comp** exibe a seguinte mensagem:

    `Files are different sizes`

    `Compare more files (Y/N)?`

    Para comparar esses arquivos, pressione N para parar o **comp** comando. Em seguida, execute novamente o **comp** com o **/n** opção a ser comparado somente a primeira parte de cada arquivo.
-   Comparando arquivos sequencialmente

    Se você usar caracteres curinga (**&#42;** e **?**) para especificar vários arquivos, **comp** localiza o primeiro arquivo que corresponde ao *Data1* e o compara com o arquivo correspondente no *Data2*, se ele existir. O **comp** comando relata os resultados da comparação para cada correspondência de arquivos *Data1*. Quando terminar, **comp** exibe a seguinte mensagem:

    `Compare more files (Y/N)?`

    Para comparar mais arquivos, pressione Y. O **comp** comando solicita os locais e nomes dos novos arquivos. Para interromper as comparações, pressione N. Quando você pressiona Y **comp** solicita a você opções de linha de comando a serem usadas. Se você não especificar nenhuma opção de linha de comando, **comp** usa que você especificou antes.

## <a name="BKMK_examples"></a>Exemplos

Comparar o conteúdo do diretório C:\Reports com o diretório de backup \\ \\Sales\Backup\April, tipo:
```
comp c:\reports \\sales\backup\april
```
Para comparar as dez primeiras linhas dos arquivos de texto no diretório \Invoice e exibir o resultado no formato decimal, digite:
```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)