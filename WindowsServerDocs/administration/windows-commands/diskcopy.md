---
title: diskcopy
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fd21efa-52cc-4e70-a7fe-35125a435106
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/07/2018
ms.openlocfilehash: 5b9343dc2f6b4c74da5a9d89a2ea804b702248cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841167"
---
# <a name="diskcopy"></a>diskcopy



Copia o conteúdo do disquete na unidade de origem em um disquete formatado ou na unidade de destino. Se usado sem parâmetros, **diskcopy** usa a unidade atual para o disco de origem e o disco de destino.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

> [!NOTE]
> Esse comando não está incluído no Windows 10.

## <a name="syntax"></a>Sintaxe

```
diskcopy [<Drive1>: [<Drive2>:]] [/v]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive1>|Especifica a unidade que contém o disco de origem.|
|\<Drive2>|Especifica a unidade que contém o disco de destino.|
|/v|Verifica se as informações foram copiadas corretamente. Essa opção reduz a velocidade o processo de cópia.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Usando discos

    **Diskcopy** funciona somente com discos removíveis, como disquetes, que deve ser do mesmo tipo. Não é possível usar **diskcopy** com um disco rígido. Se você especificar uma unidade de disco rígido para *unidade1* ou *unidade2*, **diskcopy** exibe a seguinte mensagem de erro:  
    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```  
    O **diskcopy** comando solicitará que você inserir o código-fonte e de destino de discos e aguarda até que você pressione qualquer tecla no teclado antes de continuar.

    Depois de copiar o disco **diskcopy** exibe a seguinte mensagem:  
    ```
    Copy another diskette (Y/N)?
    ```  
    Se você pressionar S, **diskcopy** solicitará a inserção de discos de origem e destino para a próxima operação de cópia. Para parar o **diskcopy** processar, pressione **N**.

    Se você estiver copiando um disquete formatado na *unidade2*, **diskcopy** formata o disco com o mesmo número de lados e setores por trilha do disco na *unidade1*. **Diskcopy** exibe a seguinte mensagem enquanto ele formata o disco e copia os arquivos:  
    ```
    Formatting while copying
    ```  
-   Números de série do disco

    Se o disco de origem tiver um número de série do volume **diskcopy** cria um novo número de série do volume para o disco de destino e exibe o número de quando a operação de cópia for concluída.
-   A omissão de parâmetros de unidade

    Se você omitir a *unidade2* parâmetro, **diskcopy** usa a unidade atual como a unidade de destino. Se você omitir os dois parâmetros de unidade **diskcopy** usa a unidade atual para ambos. Se a unidade atual for igual a *unidade1*, **diskcopy** solicitará a troca de discos, conforme necessário.
-   Usar uma única unidade para a cópia

    Execute **diskcopy** de uma unidade que não seja a unidade de disquete, por exemplo C de unidade. Se disquete *unidade1* e disquete *unidade2* são os mesmos **diskcopy** solicita que você alterne os discos. Se os discos tiverem mais informações do que a memória disponível pode conter, **diskcopy** não é possível ler todas as informações ao mesmo tempo. **Diskcopy** lê o disco de origem, grava no disco de destino e solicita que você insira o disco de origem novamente. Esse processo continua até que você copiou o disco inteiro.
-   Evitar a fragmentação do disco

    A fragmentação é a presença de pequenas áreas de espaço em disco não utilizado entre os arquivos de um disco. Um disco de origem fragmentado pode diminuir o processo de localizar, ler ou gravar arquivos.

    Porque **diskcopy** faz uma cópia exata do disco de origem no disco de destino, qualquer fragmentação do disco de origem é transferida para o disco de destino. Para evitar a transferência de fragmentação de um disco para outro, use **cópia** ou **xcopy** para copiar seu disco. Porque **cópia** e **xcopy** copiar arquivos em sequência, o novo disco não está fragmentada.

> [!NOTE]
> Não é possível usar **xcopy** para copiar um disco de inicialização.
-   Compreendendo **diskcopy** códigos de saída

    A tabela a seguir explica cada código de saída.  
    |Código de Saída|Descrição|
    |---------|-----------|
    |0|Operação de cópia foi bem-sucedida|
    |1|Ocorreu erro de leitura/gravação não fatal|
    |3|Erro fatal de disco rígido|
    |4|Erro de inicialização|

    Para processar os códigos de saída que são retornados pelo **diskcomp**, você pode usar o *ERRORLEVEL* variável de ambiente na **se** linha de comando em um arquivo em lotes.

## <a name="BKMK_examples"></a>Exemplos

Para copiar o disco da unidade B para o disco na unidade A, digite:
```
diskcopy b: a:
```
Para usar uma unidade de disquete para copiar um disquete para outro, primeiro alterne para a unidade C e, em seguida, digite:

diskcopy a: a:

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)