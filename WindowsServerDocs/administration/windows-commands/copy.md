---
title: copy
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9624d4a1-349a-4693-ad00-1d1d4e59e9ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d593fbdbffd2a5ee4e4dfb4a817ad4708162160a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853767"
---
# <a name="copy"></a>copy



Copia um ou mais arquivos de um local para outro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
copy [/d] [/v] [/n] [/y | /-y] [/z] [/a | /b] <Source> [/a | /b] [+<Source> [/a | /b] [+ ...]] [<Destination> [/a | /b]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/d|Permite que os arquivos criptografados que está sendo copiados para serem salvos como arquivos descriptografados no destino.|
|/v|Verifica se os novos arquivos são gravados corretamente.|
|/n|Usa um nome de arquivo curto, se disponível, ao copiar um arquivo com um nome mais de oito caracteres, ou com uma extensão de nome de arquivo mais de três caracteres.|
|/y|Suprime a solicitação para confirmar que você deseja substituir um arquivo de destino existente.|
|/-y|Solicita que você confirme que você deseja substituir um arquivo de destino existente.|
|/z|Copia os arquivos em rede no modo reiniciável.|
|/a|Indica um arquivo de texto ASCII.|
|/b|Indica um arquivo binário.|
|\<origem >|Obrigatório. Especifica o local do qual você deseja copiar um arquivo ou conjunto de arquivos. *Origem* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses elementos.|
|\<Destino >|Obrigatório. Especifica o local ao qual você deseja copiar um arquivo ou conjunto de arquivos. *Destino* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses elementos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Você pode copiar um arquivo de texto ASCII que usa um caractere de fim-de-arquivo (CTRL + Z) para indicar o final do arquivo.
-   Usando **/a**

    Quando **/a** precede ou segue uma lista de arquivos na linha de comando, ele se aplica a todos os arquivos listados até **cópia** encontra **/b**. Nesse caso, **/b** aplica-se o arquivo anterior **/b**.

    O efeito **/a** depende de sua posição na cadeia de caracteres de linha de comando. Quando **/a** segue *fonte*, **cópia** trata o arquivo como um arquivo ASCII e copia os dados que precedem o primeiro caractere de fim-de-arquivo (CTRL + Z).

    Quando **/a** segue *destino*, **cópia** adiciona um caractere de fim-de-arquivo (CTRL + Z) como o último caractere do arquivo.
-   Usando   **/b**

    **/ b** direciona o interpretador de comandos para ler o número de bytes especificado pelo tamanho do arquivo no diretório. **/ b** é o valor padrão para **cópia**, a menos que **cópia** combina arquivos.

    Quando **/b** precede ou segue uma lista de arquivos na linha de comando, ele se aplica a todos os arquivos listados até **cópia** encontra **/a**. Nesse caso, **/a** aplica-se o arquivo anterior **/a**.

    O efeito **/b** depende de sua posição na cadeia de caracteres de linha de comando. Quando **/b** segue *fonte*, **cópia** copia o arquivo inteiro, incluindo qualquer caractere de fim-de-arquivo (CTRL + Z).

    Quando **/b** segue *destino*, **cópia** acrescentará um caractere de fim-de-arquivo (CTRL + Z).
-   Usando **/v**

    Se uma operação de gravação não pode ser verificada por que uma mensagem de erro é exibida. Embora raramente ocorram erros de gravação com **cópia**, você pode usar **/v** para verificar se os dados críticos foram gravados corretamente. O **/v** opção de linha de comando também diminui o **cópia** de comando, porque cada setor gravado no disco deve ser verificada.
-   Usando o **/y** e   **/y**

    Se **/y** é predefinida na variável de ambiente COPYCMD, você pode substituir essa configuração usando **y/** na linha de comando. Por padrão, você será solicitado quando você substituir essa configuração, a menos que o **cópia** comando é executado em um script em lotes.
-   Anexando arquivos

    Para acrescentar arquivos, especifique um único arquivo para *destino*, mas vários arquivos para *fonte* (use caracteres curinga ou *File1* +  *Arquivo2*+*arquivo3* formato).
-   Usando **/z.**

    Se a conexão for perdida durante a fase de cópia (por exemplo, se o servidor ficar offline interrompe a conexão), **copiar /z** será retomado após a conexão for restabelecida. **/z** também exibe a porcentagem da operação de cópia for concluída para cada arquivo.
-   Copiar de e para dispositivos

    Você pode substituir o nome de uma ou mais ocorrências de um dispositivo *fonte* ou *destino*.
-   Usar ou omitir **/b** ao copiar para um dispositivo

    Quando *destino* é um dispositivo (por exemplo, Com1 ou Lpt1) **/b** copia dados para o dispositivo no modo binário. No modo binário, **copie a/b** copia todos os caracteres (incluindo caracteres especiais, como CTRL + C, CTRL + S, CTRL + Z e ENTER) para o dispositivo como dados. No entanto, se você omitir **/b**, dados são copiados para o dispositivo no modo ASCII. No modo ASCII, os caracteres especiais podem causar arquivos combine durante o processo de cópia.
-   Usando o arquivo de destino padrão

    Se você não especificar um arquivo de destino, uma cópia é criada com o mesmo nome, data de modificação e a hora do arquivo original da modificação. A nova cópia é armazenada no diretório atual na unidade atual. Se o arquivo de origem estiver na unidade atual e no diretório atual e você não especificar uma unidade diferente ou o diretório para o arquivo de destino, o **cópia** comando interrompe e exibe a seguinte mensagem de erro:

    `File cannot be copied onto itself`

    `0 File(s) copied`
-   Combinar arquivos

    Se você especificar mais de um arquivo no *fonte*, **cópia** combina tudo em um único arquivo usando o nome de arquivo especificado no *destino*. **Cópia** pressupõe que os arquivos combinados são arquivos ASCII, a menos que você use o **/b** opção.
-   Copiando arquivos de comprimento zero

    **Cópia** não copia arquivos de 0 bytes de comprimento. Use **xcopy** para copiar esses arquivos.
-   Alterando a data e hora de um arquivo

    Se você deseja atribuir a hora atual e de data para um arquivo sem modificar o arquivo, use a seguinte sintaxe:  
    ```
    copy /b <Source> +,,
    ```  
    As vírgulas indicam a omissão do *destino* parâmetro.
-   Copiando arquivos em subpastas

    Para copiar todos os arquivos e subdiretórios de um diretório, use o **xcopy** comando.
-   O **cópia** comando com parâmetros diferentes, está disponível no Console de recuperação.

## <a name="BKMK_examples"></a>Exemplos

Para copiar um arquivo denominado Memo para doc na unidade atual e certifique-se de que um caractere de fim-de-arquivo (CTRL + Z) esteja no final do arquivo copiado, digite:
```
copy memo.doc letter.doc /a
```
Para copiar um arquivo chamado typ da unidade atual e pasta para um diretório existente denominado Aves que está localizado na unidade C, digite:
```
copy robin.typ c:\birds
```
Se o diretório de pássaros não existir, o arquivo typ é copiado para um arquivo denominado Aves que está localizado no diretório raiz do disco na unidade C.

Para combinar mar89 abr89. rpt e maio89. rpt, que estão localizados no diretório atual e colocá-los em um arquivo chamado relatório (também no diretório atual), digite:
```
copy mar89.rpt + apr89.rpt + may89.rpt Report
```
Quando você combina arquivos **cópia** marcará o arquivo de destino com a data e hora atuais. Se você omitir *destino*, os arquivos são combinados e armazenados com o nome do primeiro arquivo na lista. Por exemplo, para combinar todos os arquivos no relatório quando um arquivo denominado relatório já existe, digite:
```
copy report + mar89.rpt + apr89.rpt + may89.rpt
```
Para combinar todos os arquivos no diretório atual que têm a extensão de nome de arquivo the.txt em um único arquivo chamado Combined.doc, tipo:
```
copy *.txt Combined.doc 
```
Se você quiser combinar vários arquivos binários em um arquivo usando caracteres curinga, inclua **/b**. Isso impede que o Windows trate CTRL + Z como um caractere de final de arquivo. Por exemplo, digite:
```
copy /b *.exe Combined.exe
```

> [!CAUTION]
> Se você combinar arquivos binários, o arquivo resultante pode ser inutilizável devido à formatação interna.

No exemplo a seguir **cópia** combina cada arquivo que tem uma extensão. txt com seu arquivo. ref correspondente. O resultado é um arquivo com o mesmo nome de arquivo, mas com uma extensão. doc. **Cópia** combina File1.txt com Arq1 ao formulário arquivo1 e então **cópia** combina File2.txt com Arq2 ao formulário Arq2 e assim por diante. Por exemplo, digite:
```
copy *.txt + *.ref *.doc
```
Para combinar todos os arquivos com a extensão. txt e, em seguida, combinar todos os arquivos com a extensão. ref em um arquivo chamado Combined.doc, digite:
```
copy *.txt + *.ref Combined.doc
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)