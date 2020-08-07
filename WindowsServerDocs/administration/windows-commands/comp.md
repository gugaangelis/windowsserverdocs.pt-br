---
title: comp
description: Artigo de referência para o comando comp, que compara o conteúdo de dois arquivos ou conjuntos de arquivos byte a byte.
ms.topic: article
ms.assetid: 40319d23-704d-4da1-be93-8259547275d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18bd39483957959c746913a4ee18014be40c9eaa
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880033"
---
# <a name="comp"></a>comp

Compara o conteúdo de dois arquivos ou conjuntos de arquivos byte a byte. Esses arquivos podem ser armazenados na mesma unidade ou em unidades diferentes, e no mesmo diretório ou em diretórios diferentes. Quando esse comando compara arquivos, ele exibe seus nomes de local e arquivo. Se usado sem parâmetros, **comp** solicitará que você insira os arquivos a serem comparados.

## <a name="syntax"></a>Sintaxe

```
comp [<data1>] [<data2>] [/d] [/a] [/l] [/n=<number>] [/c]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<data1>` | Especifica o local e o nome do primeiro arquivo ou conjunto de arquivos que você deseja comparar. Você pode usar caracteres curinga (**&#42;** e **?**) para especificar vários arquivos. |
| `<data2>` | Especifica o local e o nome do segundo arquivo ou conjunto de arquivos que você deseja comparar. Você pode usar caracteres curinga (**&#42;** e **?**) para especificar vários arquivos. |
| /d | Exibe as diferenças no formato decimal. (O formato padrão é hexadecimal.) |
| /a | Exibe as diferenças como caracteres. |
| /l | Exibe o número da linha em que ocorre uma diferença, em vez de exibir o deslocamento de byte. |
| /n =`<number>` | Compara somente o número de linhas que são especificadas para cada arquivo, mesmo que os arquivos sejam tamanhos diferentes. |
| /c | Executa uma comparação que não diferencia maiúsculas de minúsculas. |
| /off [linha] | Processa arquivos com o atributo offline definido. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários

- Durante a comparação, **comp** exibe mensagens que identificam os locais de informações desiguais entre os arquivos. Cada mensagem indica o endereço de deslocamento de memória dos bytes desiguais e o conteúdo dos bytes (em notação hexadecimal, a menos que o parâmetro de linha de comando **/a** ou **/d** seja especificado). As mensagens aparecem no seguinte formato:

    ```
    Compare error at OFFSET xxxxxxxx
    file1 = xx
    file2 = xx
    ```

    Depois de dez comparações desiguais, o **comp** para de comparar os arquivos e exibe a seguinte mensagem:

    `10 Mismatches - ending compare`

- Se você omitir os componentes necessários de *dados1* ou *data2*, ou se você omitir o *dados2* inteiramente, esse comando solicitará as informações ausentes.

- Se *dados1* contiver apenas uma letra da unidade ou um nome de diretório sem nome de arquivo, esse comando compara todos os arquivos no diretório especificado com o arquivo especificado em *data1*.

- Se *data2* contiver apenas uma letra de unidade ou um nome de diretório, o nome de arquivo padrão para *data2* se tornará o mesmo nome de *dados1*.

- Se o comando **comp** não conseguir localizar os arquivos especificados, ele solicitará uma mensagem sobre se você deseja comparar arquivos adicionais.

- Os arquivos que você compara podem ter o mesmo nome de arquivo, desde que estejam em diretórios diferentes ou em unidades diferentes. Você pode usar caracteres curinga (**&#42;** e **?**) para especificar nomes de arquivo.

- Você deve especificar **/n** para comparar arquivos de tamanhos diferentes. Se os tamanhos de arquivo forem diferentes e **/n** não for especificado, a seguinte mensagem será exibida:

    ```
    Files are different sizes
    Compare more files (Y/N)?
    ```

    Para comparar esses arquivos de qualquer maneira, pressione **N** para interromper o comando. Em seguida, execute o comando **comp** novamente, usando a opção **/n** para comparar apenas a primeira parte de cada arquivo.

- Se você usar caracteres curinga (**&#42;** e **?**) para especificar vários arquivos, o **comp** encontrará o primeiro arquivo que corresponde a *dados1* e o compara com o arquivo correspondente em *data2*, se existir. O comando **comp** relata os resultados da comparação para cada arquivo correspondente a *dados1*. Quando terminar, **comp** exibirá a seguinte mensagem:

    `Compare more files (Y/N)?`

    Para comparar mais arquivos, pressione **Y**. O comando **comp** solicita os locais e nomes dos novos arquivos. Para interromper as comparações, pressione **N**. Ao pressionar **s**, você será solicitado a fornecer quais opções de linha de comando usar. Se você não especificar nenhuma opção de linha de comando, **comp** usará aquelas que você especificou anteriormente.

## <a name="examples"></a>Exemplos

Para comparar o conteúdo do diretório *c:\Reports* com o diretório de backup `\\sales\backup\april` , digite:

```
comp c:\reports \\sales\backup\april
```

Para comparar as dez primeiras linhas dos arquivos de texto no diretório *\invoice* e exibir o resultado em formato decimal, digite:

```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)