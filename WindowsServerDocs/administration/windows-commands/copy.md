---
title: copy
description: Artigo de referência para o comando de cópia, que copia um ou mais arquivos de um local para outro.
ms.topic: reference
ms.assetid: 9624d4a1-349a-4693-ad00-1d1d4e59e9ac
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 129a6e4575be47ab876ef6943aeca803269ac529
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629233"
---
# <a name="copy"></a>copy

Copia um ou mais arquivos de um local para outro.

> [!NOTE]
> Você também pode usar o comando de **cópia** , com parâmetros diferentes, no console de recuperação. Para obter mais informações sobre o console de recuperação, consulte [ambiente de recuperação do Windows (Windows re)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxe

```
copy [/d] [/v] [/n] [/y | /-y] [/z] [/a | /b] <source> [/a | /b] [+<source> [/a | /b] [+ ...]] [<destination> [/a | /b]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /d | Permite que os arquivos criptografados que estão sendo copiados sejam salvos como arquivos descriptografados no destino. |
| /v | Verifica se os novos arquivos foram gravados corretamente. |
| /n | Usa um nome de arquivo curto, se disponível, ao copiar um arquivo com um nome com mais de oito caracteres ou com uma extensão de nome de arquivo com mais de três caracteres. |
| /y | Suprime a solicitação para confirmar que você deseja substituir um arquivo de destino existente. |
| /-y | Solicita que você confirme se deseja substituir um arquivo de destino existente. |
| /z | Copia arquivos em rede no modo reiniciável. |
| /a | Indica um arquivo de texto ASCII. |
| /b | Indica um arquivo binário. |
| `<source>` | Obrigatórios. Especifica o local do qual você deseja copiar um arquivo ou conjunto de arquivos. A *origem* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses. |
| `<destination>` | Obrigatórios. Especifica o local para o qual você deseja copiar um arquivo ou conjunto de arquivos. O *destino* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Você pode copiar um arquivo de texto ASCII que usa um caractere de final de arquivo (CTRL + Z) para indicar o final do arquivo.

- Se **/a** precede ou segue uma lista de arquivos na linha de comando, ele se aplica a todos os arquivos listados até que a **cópia** encontre **/b**. Nesse caso, **/b** aplica-se ao arquivo anterior a **/b**.

    O efeito de **/a** depende de sua posição na cadeia de caracteres de linha de comando:
      - Se **/a** seguir *Source*, o comando **Copy** tratará o arquivo como um arquivo ASCII e copiará os dados que antecedem o primeiro caractere de fim de arquivo (Ctrl + Z).
      - Se **/a** seguir o *destino*, o comando de **cópia** adicionará um caractere de final de arquivo (Ctrl + Z) como o último caractere do arquivo.

- Se **/b** direciona o interpretador de comandos para ler o número de bytes especificado pelo tamanho do arquivo no diretório. **/b** é o valor padrão para **cópia**, a menos que a **cópia** Combine arquivos.

- Se **/b** precede ou segue uma lista de arquivos na linha de comando, ele se aplica a todos os arquivos listados até que a **cópia** encontre **/a**. Nesse caso, **/a** aplica-se ao arquivo anterior a **/a**.

    O efeito de **/b** depende de sua posição na cadeia de caracteres de linha de comando:-se **/b** seguir a *origem*, o comando de **cópia** copiará o arquivo inteiro, incluindo qualquer caractere de fim de arquivo (Ctrl + Z).
        -Se **/b** seguir o *destino*, o comando de **cópia** não adicionará um caractere de fim de arquivo (Ctrl + Z).

- Se uma operação de gravação não puder ser verificada, uma mensagem de erro será exibida. Embora os erros de gravação raramente ocorram com o comando de **cópia** , você pode usar **/v** para verificar se os dados críticos foram registrados corretamente. A opção de linha de comando **/v** também retarda o comando de **cópia** , pois cada setor registrado no disco deve ser verificado.

- Se **/y** for predefinido na variável de ambiente **COPYCMD** , você poderá substituir essa configuração usando **/-y** na linha de comando. Por padrão, você receberá uma solicitação quando substituir essa configuração, a menos que o comando de **cópia** seja executado em um script em lote.

- Para acrescentar arquivos, especifique um único arquivo para *destino*, mas vários arquivos para *origem* (use caracteres curinga ou o formato *arquivo1* + *arquivo2* + *arquivo3* ).

- Se a conexão for perdida durante a fase de cópia (por exemplo, se o servidor ficar offline interromper a conexão), você poderá usar **Copy/z** para continuar depois que a conexão for restabelecida. A opção **/z** também exibe a porcentagem da operação de cópia concluída para cada arquivo.

- Você pode substituir um nome de dispositivo por uma ou mais ocorrências de *origem* ou *destino*.

- Se o *destino* for um dispositivo (por exemplo, COM1 ou LPT1), a opção **/b** copiará os dados para o dispositivo no modo binário. No modo binário, **copy/b** copia todos os caracteres (incluindo caracteres especiais, como CTRL + C, CTRL + S, CTRL + Z e Enter) para o dispositivo, como dados. No entanto, se você omitir **/b**, os dados serão copiados para o dispositivo no modo ASCII. No modo ASCII, os caracteres especiais podem fazer com que os arquivos sejam combinados durante o processo de cópia.

- Se você não especificar um arquivo de destino, uma cópia será criada com o mesmo nome, data de modificação e hora de modificação que o arquivo original. A nova cópia é armazenada no diretório atual na unidade atual. Se o arquivo de origem estiver na unidade atual e no diretório atual e você não especificar uma unidade ou um diretório diferente para o arquivo de destino, o comando de **cópia** será interrompido e exibirá a seguinte mensagem de erro:

    ```
    File cannot be copied onto itself
    0 File(s) copied
    ```

- Se você especificar mais de um arquivo na *origem*, o comando de **cópia** os combinará em um único arquivo usando o nome de arquivo especificado no *destino*. O comando de **cópia** assume que os arquivos combinados são arquivos ASCII, a menos que você use a opção **/b** .

- Para copiar arquivos com 0 bytes de comprimento ou para copiar todos os arquivos e subdiretórios de um diretório, use o [comando xcopy](xcopy.md).

- Para atribuir a data e a hora atuais a um arquivo sem modificar o arquivo, use a seguinte sintaxe:

    ```
    copy /b <source> +,,
    ```

    Onde as vírgulas indicam que o parâmetro de *destino* foi intencionalmente saiu.

## <a name="examples"></a>Exemplos

Para copiar um arquivo chamado *memo.doc* para *letter.doc* na unidade atual e garantir que um caractere de fim de arquivo (Ctrl + Z) esteja no final do arquivo copiado, digite:

```
copy memo.doc letter.doc /a
```

Para copiar um arquivo chamado *Robin. Typ* da unidade atual e do diretório para um diretório existente chamado *pássaros* que está localizado na unidade C, digite:

```
copy robin.typ c:\birds
```

> [!NOTE]
> Se o diretório de *pássaros* não existir, o arquivo *Robin. Typ* será copiado em um arquivo chamado *pássaros* que está localizado no diretório raiz do disco na unidade C.

Para combinar *Mar89. RPT*, *Apr89. RPT*e *May89. RPT*, que estão localizados no diretório atual e colocá-los em um arquivo chamado *Report* (também no diretório atual), digite:

```
copy mar89.rpt + apr89.rpt + may89.rpt Report
```

> [!NOTE]
> Se você combinar arquivos, o comando de **cópia** marcará o arquivo de destino com a data e a hora atuais. Se você omitir o *destino*, os arquivos serão combinados e armazenados no nome do primeiro arquivo na lista.

Para combinar todos os arquivos no *relatório*, quando um arquivo chamado *relatório* já existir, digite:

```
copy report + mar89.rpt + apr89.rpt + may89.rpt
```

Para combinar todos os arquivos no diretório atual que têm a extensão de nome de arquivo. txt em um único arquivo chamado *Combined.doc*, digite:

```
copy *.txt Combined.doc
```

Para combinar vários arquivos binários em um arquivo usando caracteres curinga, inclua **/b**. Isso impede que o Windows trate CTRL + Z como um caractere de final de arquivo. Por exemplo, digite:

```
copy /b *.exe Combined.exe
```

> [!CAUTION]
> Se você combinar arquivos binários, o arquivo resultante poderá ser inutilizável devido à formatação interna.

- A combinação de cada arquivo que tem uma extensão. txt com seu arquivo. ref correspondente cria um arquivo com o mesmo nome de arquivo, mas com uma extensão. doc. O comando de **cópia** combina *file1.txt* com *arquivo1. ref* para o formulário *file1.doc*e, em seguida, o comando combina *file2.txt* com *arquivo2. ref* ao formulário *file2.doc*e assim por diante. Por exemplo, digite:

```
copy *.txt + *.ref *.doc
```

Para combinar todos os arquivos com a extensão. txt e, em seguida, combinar todos os arquivos com a extensão. ref em um arquivo chamado *Combined.doc*, digite:

```
copy *.txt + *.ref Combined.doc
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando xcopy](xcopy.md)
