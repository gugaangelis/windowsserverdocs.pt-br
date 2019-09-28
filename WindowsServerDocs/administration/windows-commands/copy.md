---
title: copy
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 102fd6b59516b04b8986ee47b52f521be73f04de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379043"
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
|/d|Permite que os arquivos criptografados que estão sendo copiados sejam salvos como arquivos descriptografados no destino.|
|/v|Verifica se os novos arquivos foram gravados corretamente.|
|opção|Usa um nome de arquivo curto, se disponível, ao copiar um arquivo com um nome com mais de oito caracteres ou com uma extensão de nome de arquivo com mais de três caracteres.|
|/y|Suprime a solicitação para confirmar que você deseja substituir um arquivo de destino existente.|
|/-y|Solicita que você confirme se deseja substituir um arquivo de destino existente.|
|/z|Copia arquivos em rede no modo reiniciável.|
|SRDF|Indica um arquivo de texto ASCII.|
|/b.|Indica um arquivo binário.|
|\<Source >|Obrigatório. Especifica o local do qual você deseja copiar um arquivo ou conjunto de arquivos. A *origem* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses.|
|\<Destination >|Obrigatório. Especifica o local para o qual você deseja copiar um arquivo ou conjunto de arquivos. O *destino* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Você pode copiar um arquivo de texto ASCII que usa um caractere de final de arquivo (CTRL + Z) para indicar o final do arquivo.
-   Usando **/a**

    Quando **/a** precede ou segue uma lista de arquivos na linha de comando, ele se aplica a todos os arquivos listados até que a **cópia** encontre **/b**. Nesse caso, **/b** aplica-se ao arquivo anterior a **/b**.

    O efeito de **/a** depende de sua posição na cadeia de caracteres de linha de comando. Quando **/a** segue a *origem*, a **cópia** trata o arquivo como um arquivo ASCII e copia os dados que precede o primeiro caractere de fim de arquivo (Ctrl + Z).

    Quando **/a** segue o *destino*, **Copy** adiciona um caractere de final de arquivo (Ctrl + Z) como o último caractere do arquivo.
-   Usando **/b**

    **/b** direciona o interpretador de comandos para ler o número de bytes especificado pelo tamanho do arquivo no diretório. **/b** é o valor padrão para **cópia**, a menos que a **cópia** Combine arquivos.

    Quando **/b** precede ou segue uma lista de arquivos na linha de comando, ele se aplica a todos os arquivos listados até que a **cópia** encontre **/a**. Nesse caso, **/a** aplica-se ao arquivo anterior a **/a**.

    O efeito de **/b** depende de sua posição na cadeia de caracteres de linha de comando. Quando **/b** segue a *origem*, **Copie** copia o arquivo inteiro, incluindo qualquer caractere de fim de arquivo (Ctrl + Z).

    Quando **/b** segue o *destino*, **Copy** não adiciona um caractere de final de arquivo (Ctrl + Z).
-   Usando **/v**

    Se uma operação de gravação não puder ser verificada, uma mensagem de erro será exibida. Embora os erros de gravação raramente ocorram com a **cópia**, você pode usar **/v** para verificar se os dados críticos foram registrados corretamente. A opção de linha de comando **/v** também retarda o comando de **cópia** , pois cada setor registrado no disco deve ser verificado.
-   Usando **/y** e **/-y**

    Se **/y** for predefinido na variável de ambiente COPYCMD, você poderá substituir essa configuração usando **/-y** na linha de comando. Por padrão, você receberá uma solicitação quando substituir essa configuração, a menos que o comando de **cópia** seja executado em um script em lote.
-   Anexando arquivos

    Para acrescentar arquivos, especifique um único arquivo para o *destino*, mas vários arquivos para *origem* (use caracteres curinga ou *arquivo1*+*arquivo2*+ formato*arquivo3* ).
-   Usando **/z**

    Se a conexão for perdida durante a fase de cópia (por exemplo, se o servidor ficar offline interromper a conexão), **Copy/z** retomará após a conexão ser restabelecida. **/z** também exibe a porcentagem da operação de cópia concluída para cada arquivo.
-   Copiando de e para dispositivos

    Você pode substituir um nome de dispositivo por uma ou mais ocorrências de *origem* ou *destino*.
-   Usando ou omitindo **/b** ao copiar para um dispositivo

    Quando o *destino* é um dispositivo (por exemplo, COM1 ou LPT1), **/b** copia os dados para o dispositivo no modo binário. No modo binário, **copy/b** copia todos os caracteres (incluindo caracteres especiais, como CTRL + C, CTRL + S, CTRL + Z e Enter) para o dispositivo como dados. No entanto, se você omitir **/b**, os dados serão copiados para o dispositivo no modo ASCII. No modo ASCII, os caracteres especiais podem fazer com que os arquivos sejam combinados durante o processo de cópia.
-   Usando o arquivo de destino padrão

    Se você não especificar um arquivo de destino, uma cópia será criada com o mesmo nome, data de modificação e hora de modificação que o arquivo original. A nova cópia é armazenada no diretório atual na unidade atual. Se o arquivo de origem estiver na unidade atual e no diretório atual e você não especificar uma unidade ou um diretório diferente para o arquivo de destino, o comando de **cópia** será interrompido e exibirá a seguinte mensagem de erro:

    `File cannot be copied onto itself`

    `0 File(s) copied`
-   Combinando arquivos

    Se você especificar mais de um arquivo na *origem*, a **cópia** os combinará em um único arquivo usando o nome de arquivo especificado no *destino*. A **cópia** pressupõe que os arquivos combinados são arquivos ASCII, a menos que você use a opção **/b** .
-   Copiando arquivos de comprimento zero

    **Copy** não copia arquivos que tenham 0 bytes de comprimento. Use **xcopy** para copiar esses arquivos.
-   Alterando a data e a hora de um arquivo

    Se você quiser atribuir a data e a hora atuais a um arquivo sem modificar o arquivo, use a seguinte sintaxe:  
    ```
    copy /b <Source> +,,
    ```  
    As vírgulas indicam a omissão do parâmetro de *destino* .
-   Copiando arquivos em subdiretórios

    Para copiar todos os arquivos e subdiretórios de um diretório, use o comando **xcopy** .
-   O comando de **cópia** , com parâmetros diferentes, está disponível no console de recuperação.

## <a name="BKMK_examples"></a>Disso

Para copiar um arquivo chamado Memo. doc para o Letter. doc na unidade atual e garantir que um caractere de fim de arquivo (CTRL + Z) esteja no final do arquivo copiado, digite:
```
copy memo.doc letter.doc /a
```
Para copiar um arquivo chamado Robin. Typ da unidade atual e do diretório para um diretório existente chamado pássaros que está localizado na unidade C, digite:
```
copy robin.typ c:\birds
```
Se o diretório de pássaros não existir, o arquivo Robin. Typ será copiado em um arquivo chamado pássaros que está localizado no diretório raiz do disco na unidade C.

Para combinar Mar89. RPT, Apr89. RPT e May89. RPT, que estão localizados no diretório atual e colocá-los em um arquivo chamado Report (também no diretório atual), digite:
```
copy mar89.rpt + apr89.rpt + may89.rpt Report
```
Ao combinar arquivos, **Copie** marca o arquivo de destino com a data e a hora atuais. Se você omitir o *destino*, os arquivos serão combinados e armazenados no nome do primeiro arquivo na lista. Por exemplo, para combinar todos os arquivos no relatório quando um arquivo chamado relatório já existir, digite:
```
copy report + mar89.rpt + apr89.rpt + may89.rpt
```
Para combinar todos os arquivos no diretório atual que têm a extensão de nome de arquivo. txt em um único arquivo chamado combinado. doc, digite:
```
copy *.txt Combined.doc 
```
Se você quiser combinar vários arquivos binários em um arquivo usando caracteres curinga, inclua **/b**. Isso impede que o Windows trate CTRL + Z como um caractere de final de arquivo. Por exemplo, digite:
```
copy /b *.exe Combined.exe
```

> [!CAUTION]
> Se você combinar arquivos binários, o arquivo resultante poderá ser inutilizável devido à formatação interna.

No exemplo a seguir, a **cópia** combina cada arquivo que tem uma extensão. txt com seu arquivo. ref correspondente. O resultado é um arquivo com o mesmo nome de arquivo, mas com uma extensão. doc. A **cópia** combina arquivo1. txt com arquivo1. ref para o formulário arquivo1. doc e, em seguida, a **cópia** combina arquivo2. txt com file2. ref para o formulário file2. doc e assim por diante. Por exemplo, digite:
```
copy *.txt + *.ref *.doc
```
Para combinar todos os arquivos com a extensão. txt e, em seguida, combinar todos os arquivos com a extensão. ref em um arquivo chamado combinado. doc, digite:
```
copy *.txt + *.ref Combined.doc
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)