---
title: diskcopy
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 553a85ac4fd9b7708d7adc668be4e000b36a9346
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377823"
---
# <a name="diskcopy"></a>diskcopy



Copia o conteúdo do disquete na unidade de origem para um disquete formatado ou não formatado na unidade de destino. Se usado sem parâmetros, **diskcopy** usa a unidade atual para o disco de origem e o disco de destino.

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
|\<Drive1 >|Especifica a unidade que contém o disco de origem.|
|\<Drive2 >|Especifica a unidade que contém o disco de destino.|
|/v|Verifica se as informações foram copiadas corretamente. Essa opção reduz o processo de cópia.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Usando discos

    O **diskcopy** funciona somente com discos removíveis, como disquetes, que devem ser do mesmo tipo. Você não pode usar o **diskcopy** com um disco rígido. Se você especificar uma unidade de disco rígido para *unidade1* ou *unidade2*, **diskcopy** exibirá a seguinte mensagem de erro:  
    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```  
    O comando **diskcopy** solicita que você insira os discos de origem e de destino e aguarda que você pressione qualquer tecla no teclado antes de continuar.

    Depois de copiar o disco, **diskcopy** exibe a seguinte mensagem:  
    ```
    Copy another diskette (Y/N)?
    ```  
    Se você pressionar Y, **diskcopy** solicitará que você insira discos de origem e de destino para a próxima operação de cópia. Para interromper o processo do **diskcopy** , pressione **N**.

    Se você estiver copiando para um disquete não formatado em *unidade2*, **diskcopy** formatará o disco com o mesmo número de lados e setores por faixa, como estão no disco em *unidade1*. **Diskcopy** exibe a mensagem a seguir enquanto formata o disco e copia os arquivos:  
    ```
    Formatting while copying
    ```  
-   Números de série do disco

    Se o disco de origem tiver um número de série de volume, o **diskcopy** criará um novo número de série de volume para o disco de destino e exibirá o número quando a operação de cópia for concluída.
-   Omitindo parâmetros de unidade

    Se você omitir o parâmetro *unidade2* , **diskcopy** usará a unidade atual como a unidade de destino. Se você omitir ambos os parâmetros de unidade, **diskcopy** usará a unidade atual para ambos. Se a unidade atual for igual a *unidade1*, **diskcopy** solicitará que você troque os discos conforme necessário.
-   Usando uma unidade para copiar

    Execute o **diskcopy** em uma unidade que não seja a unidade de disquete, por exemplo, a unidade C. Se o disquete *unidade1* e o disquete *unidade2* forem iguais, **diskcopy** solicitará que você alterne os discos. Se os discos contiverem mais informações do que a memória disponível pode conter, o **diskcopy** não poderá ler todas as informações ao mesmo tempo. **Diskcopy** lê do disco de origem, grava no disco de destino e solicita que você insira o disco de origem novamente. Esse processo continua até que você tenha copiado todo o disco.
-   Evitando a fragmentação de disco

    A fragmentação é a presença de áreas pequenas de espaço em disco não utilizado entre os arquivos existentes em um disco. Um disco de origem fragmentado pode retardar o processo de localizar, ler ou gravar arquivos.

    Como **diskcopy** faz uma cópia exata do disco de origem no disco de destino, qualquer fragmentação no disco de origem é transferida para o disco de destino. Para evitar a transferência de fragmentação de um disco para outro, use **Copy** ou **xcopy** para copiar o disco. Como **Copy** e **xcopy** copiam arquivos sequencialmente, o novo disco não está fragmentado.

> [!NOTE]
> Você não pode usar **xcopy** para copiar um disco de inicialização.
> -   Compreendendo os códigos de saída do **diskcopy**

    The following table explains each exit code.  
    |Código de Saída|Descrição|
    |---------|-----------|
    |0|A operação de cópia foi bem-sucedida|
    |1|Ocorreu um erro de leitura/gravação não fatal|
    |3|Ocorreu um erro de hardware fatal|
    |4|Ocorreu um erro de inicialização|

    To process the exit codes that are returned by **diskcomp**, you can use the *ERRORLEVEL* environment variable on the **if** command line in a batch program.

## <a name="BKMK_examples"></a>Disso

Para copiar o disco na unidade B para o disco na unidade A, digite:
```
diskcopy b: a:
```
Para usar a unidade de disquete a para copiar um disquete para outro, primeiro alterne para a unidade C e, em seguida, digite:

diskcopy a: a:

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)