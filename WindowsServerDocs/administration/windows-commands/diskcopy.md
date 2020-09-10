---
title: diskcopy
description: Artigo de referência para o comando diskcopy, que copia o conteúdo do disquete na unidade de origem para um disquete formatado ou não formatado na unidade de destino.
ms.topic: reference
ms.assetid: 5fd21efa-52cc-4e70-a7fe-35125a435106
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 05/07/2018
ms.openlocfilehash: 5ed6c3eee6f096e6069e5752441229015db8ed86
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634956"
---
# <a name="diskcopy"></a>diskcopy

Copia o conteúdo do disquete na unidade de origem para um disquete formatado ou não formatado na unidade de destino. Se usado sem parâmetros, **diskcopy** usa a unidade atual para o disco de origem e o disco de destino.

## <a name="syntax"></a>Sintaxe

```
diskcopy [<drive1>: [<drive2>:]] [/v]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<drive1>` | Especifica a unidade que contém o disco de origem. |
| /v | Verifica se as informações foram copiadas corretamente. Essa opção reduz o processo de cópia. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- O **diskcopy** funciona somente com discos removíveis, como disquetes, que devem ser do mesmo tipo. Você não pode usar o **diskcopy** com um disco rígido. Se você especificar uma unidade de disco rígido para *unidade1* ou *unidade2*, **diskcopy** exibirá a seguinte mensagem de erro:

    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```

    O comando **diskcopy** solicita que você insira os discos de origem e de destino e aguarda que você pressione qualquer tecla no teclado antes de continuar.

    Depois de copiar o disco, **diskcopy** exibe a seguinte mensagem:

    ```
    Copy another diskette (Y/N)?
    ```

    Se você pressionar **Y**, **diskcopy** solicitará que você insira discos de origem e de destino para a próxima operação de cópia. Para interromper o processo do **diskcopy** , pressione **N**.

    Se você estiver copiando para um disquete não formatado em *unidade2*, **diskcopy** formatará o disco com o mesmo número de lados e setores por faixa, como estão no disco em *unidade1*. **Diskcopy** exibe a mensagem a seguir enquanto formata o disco e copia os arquivos:

    ```
    Formatting while copying
    ```

- Se o disco de origem tiver um número de série de volume, o **diskcopy** criará um novo número de série de volume para o disco de destino e exibirá o número quando a operação de cópia for concluída.

- Se você omitir o parâmetro *unidade2* , **diskcopy** usará a unidade atual como a unidade de destino. Se você omitir ambos os parâmetros de unidade, **diskcopy** usará a unidade atual para ambos. Se a unidade atual for igual a *unidade1*, **diskcopy** solicitará que você troque os discos conforme necessário.

- Execute o **diskcopy** em uma unidade que não seja a unidade de disquete, por exemplo, a unidade C. Se o disquete *unidade1* e o disquete *unidade2* forem iguais, **diskcopy** solicitará que você alterne os discos. Se os discos contiverem mais informações do que a memória disponível pode conter, o **diskcopy** não poderá ler todas as informações ao mesmo tempo. **Diskcopy** lê do disco de origem, grava no disco de destino e solicita que você insira o disco de origem novamente. Esse processo continua até que você tenha copiado todo o disco.

- A fragmentação é a presença de áreas pequenas de espaço em disco não utilizado entre os arquivos existentes em um disco. Um disco de origem fragmentado pode retardar o processo de localizar, ler ou gravar arquivos.

    Como **diskcopy** faz uma cópia exata do disco de origem no disco de destino, qualquer fragmentação no disco de origem é transferida para o disco de destino. Para evitar a transferência de fragmentação de um disco para outro, use o [comando de cópia](copy.md) ou o [comando xcopy](xcopy.md) para copiar o disco. Como **Copy** e **xcopy** copiam arquivos sequencialmente, o novo disco não está fragmentado.

    > [!NOTE]
    > Você não pode usar **xcopy** para copiar um disco de inicialização.

- códigos de saída de **diskcopy** :

    | Código de saída | Descrição |
    | --------- | ----------- |
    | 0 | A operação de cópia foi bem-sucedida |
    | 1 | Ocorreu um erro de leitura/gravação não fatal |
    | 3 | Ocorreu um erro de hardware fatal |
    | 4 | Ocorreu um erro de inicialização |

    Para processar os códigos de saída retornados por **diskcomp**, você pode usar a variável de ambiente *ERRORLEVEL* na linha de comando **If** em um programa em lotes.

## <a name="examples"></a>Exemplos

Para copiar o disco na unidade B para o disco na unidade A, digite:

```
diskcopy b: a:
```

Para usar a unidade de disquete a para copiar um disquete para outro, primeiro alterne para a unidade C e, em seguida, digite:

```
diskcopy a: a:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando xcopy](xcopy.md)

- [comando de cópia](copy.md)
