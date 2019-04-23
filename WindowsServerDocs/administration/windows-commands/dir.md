---
title: dir
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: edcbf69b-eaa4-466e-b210-3dd8892f4d93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d11a2d149ec1d83facd4aea64019bbb963ec70e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840677"
---
# <a name="dir"></a>dir



Exibe uma lista de arquivos e subdiretórios de um diretório. Se usado sem parâmetros, **dir** exibe o rótulo do volume do disco e o número de série, seguido por uma lista de diretórios e arquivos no disco (incluindo seus nomes e a data e hora que cada um foi modificado pela última vez). Para arquivos, **dir** exibe a extensão de nome e o tamanho em bytes. **Dir** também exibe o número total de arquivos e diretórios listados, seu tamanho cumulativo e o espaço livre (em bytes) restante no disco.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
dir [<Drive>:][<Path>][<FileName>] [...] [/p] [/q] [/w] [/d] [/a[[:]<Attributes>]][/o[[:]<SortOrder>]] [/t[[:]<TimeField>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive>:][<Path>]|Especifica a unidade e diretório para o qual você deseja ver uma listagem.|
|[\<FileName>]|Especifica um determinado arquivo ou grupo de arquivos para o qual você deseja ver uma listagem.|
|/p|Exibe uma tela de lista por vez. Para ver a próxima tela, pressione qualquer tecla no teclado.|
|/q|Exibe informações de propriedade.|
|/w|Exibe a lista em formato amplo, com até cinco nomes de arquivo ou nomes de diretório em cada linha.|
|/d|Exibe a listagem no mesmo formato que **/w**, mas os arquivos são classificados pela coluna.|
|/a[[:]\<Attributes>]|Exibe apenas os nomes das pastas e arquivos com os atributos que você especificar. Se você omitir **/a**, **dir** exibe os nomes de todos os arquivos, exceto ocultos e arquivos do sistema. Se você usar **/a** sem especificar *atributos*, **dir** exibe os nomes de todos os arquivos, inclusive arquivos ocultados e sistema.</br>A lista a seguir descreve cada um dos valores que você pode usar para *atributos*. Usando dois-pontos (:) é opcional. Use qualquer combinação desses valores e não separe os valores com espaços.</br>**1!d** diretórios</br>**h** arquivos ocultos</br>**s** arquivos do sistema</br>**l** pontos de nova análise</br>**r** arquivos somente leitura</br>**um** arquivos prontos para arquivamento</br>**Eu** não indexadas de arquivos de conteúdo</br>**-** Prefixo que significa "não"|
|/o[[:]\<SortOrder>]|Classifica a saída de acordo com a *SortOrder*, que pode ser qualquer combinação dos valores a seguir:</br>**n** por nome (em ordem alfabética)</br>**e** por extensão (em ordem alfabética)</br>**g** grupo diretórios primeiro</br>**s** por tamanho (ordem crescente)</br>**1!d** por data/hora (primeiro o mais antigo)</br>**-** Prefixo para inverter a ordem</br>Observação: Usar dois-pontos é opcional. Vários valores são processados na ordem em que você listá-los. Não Separe vários valores com espaços.</br>Se *SortOrder* não for especificado, **/o dir** lista os diretórios em ordem alfabética, seguido de arquivos, que também são classificados em ordem alfabética.|
|/t[[:]\<TimeField>]|Especifica qual campo de tempo para exibir ou usada para classificação. A lista a seguir descreve cada um dos valores que você pode usar para *hora*:</br>**c** criação</br>**um** último acesso</br>**w** gravado pela última vez|
|/s|Lista todas as ocorrências do nome do arquivo especificado dentro do diretório especificado e todos os subdiretórios.|
|/b|Exibe uma lista de depender de diretórios e arquivos, sem informações adicionais. **/ b** substituições **/w**.|
|/l|Exibe nomes de diretório e nomes de arquivo em letras minúsculas não ordenados.|
|/n|Exibe um formato de lista longos com nomes de arquivo na extrema direita da tela.|
|/x|Exibe os nomes curtos gerados para os nomes de arquivo não 8dot3. A exibição é o mesmo que a exibição para **/n**, mas o nome curto é inserido antes do nome completo.|
|/c|Exibe o separador de milhar em tamanhos de arquivo. Esse é o comportamento padrão. Use **c/** para ocultar separadores.|
|/4|Exibe os anos em formato de quatro dígitos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para usar várias *FileName* parâmetros, separe cada nome de arquivo com um espaço, vírgula ou ponto e vírgula.
-   Você pode usar caracteres curinga (**&#42;** ou **?**), para representar um ou mais caracteres do nome do arquivo e exibir um subconjunto de arquivos ou subdiretórios.

    **Asterisco (\*):** Use o asterisco como um substituto para qualquer cadeia de caracteres, por exemplo:  
    -   **dir \*. txt** lista todos os arquivos no diretório atual com as extensões que começam com. txt, como. txt, .txt1, .txt_old.
    -   **dir de leitura\*. txt** lista todos os arquivos no diretório atual que começam com "leitura" e com as extensões que começam com. txt, como. txt, .txt1 ou .txt_old.
    -   **dir de leitura\*.\***  lista todos os arquivos no diretório atual que começam com "leitura" com qualquer extensão.

    O caractere curinga asterisco sempre usa mapeamento de nomes de arquivos curtos, portanto, você pode obter resultados inesperados. Por exemplo, o seguinte diretório contém dois arquivos (t.txt2 e t97.txt):  
    ```
    C:\test>dir /x
    Volume in drive C has no label.
    Volume Serial Number is B86A-EF32
    
    Directory of C:\test
    
    11/30/2004  01:40 PM <DIR>  .
    11/30/2004  01:40 PM <DIR> ..
    11/30/2004  11:05 AM 0 T97B4~1.TXT t.txt2
    11/30/2004  01:16 PM 0 t97.txt
    ```  
    Você pode esperar que pressionar **dir t97\***  retornaria t97.txt o arquivo. No entanto, digitar **dir t97\***  retorna os dois arquivos, como o caractere curinga asterisco corresponde a t.txt2 de arquivo para t97.txt usando seu mapa de nome curto T97B4~1.TXT. Da mesma forma, digitar **del t97\***  excluiria os dois arquivos.

    **Ponto de interrogação (?):** Use o ponto de interrogação como um substituto para um único caractere em um nome. Por exemplo, digitar **dir ler???. txt** lista todos os arquivos no diretório atual com a extensão. txt que começam com "leitura" e são seguidos por até três caracteres. Isso inclui Read.txt, Read1.txt, Read12.txt, Read123.txt e Readme1.txt, mas não Readme12.txt.
-   Especificando atributos de exibição de arquivo

    Se você usar **/a** com mais de um valor em *atributos*, **dir** exibe os nomes dos somente os arquivos com todos os atributos especificados. Por exemplo, se você usar **/a** com **r** e **-h** como atributos (usando o **/a: r-h** ou **/ar-h** ), **dir** será somente exibirá os nomes dos arquivos somente leitura que não estão ocultos.
-   Especificando a classificação de nome de arquivo

    Se você especificar mais de uma *SortOrder* valor, **dir** classifica os nomes de arquivo pelo primeiro critério e, em seguida, pelo segundo critério e assim por diante. Por exemplo, se você usar **/o** com o **eletrônico** e **-s** os valores para *SortOrder* (usando o **/o: e-s**ou **/oe-s**), **dir** classificará os nomes de diretórios e arquivos por extensão, com o maior em primeiro lugar e, em seguida, exibe o resultado final. A classificação alfabética por extensão faz com que os nomes de arquivos sem extensões apareçam primeiro, nomes de diretório e, em seguida, nomes de arquivos com extensões.
-   Usando pipes e símbolos de redirecionamento

    Quando você usa o símbolo de redirecionamento (**>**) para enviar **dir** de saída para um arquivo ou um pipe (**|**) para enviar **dir**de saída para outro comando, use **/a:-d** e **/b** para listar os nomes de arquivo. Você pode usar *FileName* com **/b** e **/s** para especificar que **dir** é pesquisar o diretório atual e seus subdiretórios para todos os arquivos nomes que correspondem *FileName*. **Dir** lista apenas a letra da unidade, nome do diretório, nome de arquivo e extensão de nome de arquivo (um caminho por linha) para cada arquivo nome localiza. Antes de usar um pipe envie **dir** de saída para outro comando, você deve definir o TEMP variável de ambiente em seu arquivo Autoexec.
-   O **dir** comando com parâmetros diferentes, está disponível no Console de recuperação.

## <a name="BKMK_examples"></a>Exemplos

Para exibir todos os diretórios um após o outro, em ordem alfabética, no formato amplo e pausar depois de cada tela, certifique-se de que o diretório raiz é o diretório atual e, em seguida, digite:
```
dir /s/w/o/p
```
**Dir** lista o diretório raiz, as subpastas e os arquivos no diretório raiz, incluindo as extensões. Em seguida, **dir** lista os nomes de subpastas e nomes de arquivo em cada subdiretório na árvore.

Para alterar o exemplo anterior, de modo que **dir** exibe os nomes de arquivo e extensões, mas omite os nomes de diretório, digite:
```
dir /s/w/o/p/a:-d
```
Para imprimir uma listagem de diretório, digite:
```
dir > prn
```
Quando você especifica **prn**, a lista de diretórios é enviada para a impressora que está conectada à porta LPT1. Se sua impressora está conectada a uma porta diferente, você deve substituir **prn** com o nome da porta correta.

Você também pode redirecionar a saída do **dir** comando para um arquivo, substituindo **prn** com um nome de arquivo. Você também pode digitar um caminho. Por exemplo, para o direct **dir** para Regs o arquivo no diretório de registros, o tipo de saída:
```
dir > \records\dir.doc
```
Se não existir, Regs **dir** cria, a menos que o diretório de registros não existe. Nesse caso, a seguinte mensagem será exibida:

`File creation error`

Para exibir uma lista de todos os nomes de arquivo com a extensão. txt em todos os diretórios na unidade C, digite:
```
dir c:\*.txt /w/o/s/p
```
**Dir** exibe, em formato amplo, nomes de uma lista em ordem alfabética de arquivo correspondente em cada diretório e ele faz uma pausa sempre que a tela é completada até que você pressione qualquer tecla para continuar.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)