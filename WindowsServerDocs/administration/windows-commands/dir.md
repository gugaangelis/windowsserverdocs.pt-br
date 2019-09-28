---
title: dir
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b073b0557cd011f6742a8a8e532165f53b0a6974
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377881"
---
# <a name="dir"></a>dir



Exibe uma lista de arquivos e subdiretórios de um diretório. Se usado sem parâmetros, **dir** exibirá o rótulo de volume e o número de série do disco, seguido por uma lista de diretórios e arquivos no disco (incluindo seus nomes e a data e hora em que cada foi modificado pela última vez). Para arquivos, **dir** exibe a extensão de nome e o tamanho em bytes. **Dir** também exibe o número total de arquivos e diretórios listados, seu tamanho cumulativo e o espaço livre (em bytes) restante no disco.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
dir [<Drive>:][<Path>][<FileName>] [...] [/p] [/q] [/w] [/d] [/a[[:]<Attributes>]][/o[[:]<SortOrder>]] [/t[[:]<TimeField>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive >:] [<Path>]|Especifica a unidade e o diretório para os quais você deseja ver uma listagem.|
|[\<FileName >]|Especifica um arquivo ou grupo de arquivos específico para o qual você deseja ver uma listagem.|
|/p|Exibe uma tela da listagem de cada vez. Para ver a próxima tela, pressione qualquer tecla no teclado.|
|/q|Exibe informações de Propriedade do arquivo.|
|/w|Exibe a lista em formato largo, com até cinco nomes de arquivo ou nomes de diretório em cada linha.|
|/d|Exibe a listagem no mesmo formato que **/w**, mas os arquivos são classificados por coluna.|
|/a [[:] \<Attributes >]|Exibe somente os nomes desses diretórios e arquivos com os atributos que você especificar. Se você omitir **/a**, **dir** exibirá os nomes de todos os arquivos, exceto arquivos ocultos e do sistema. Se você usar **/a** sem especificar *atributos*, **dir** exibirá os nomes de todos os arquivos, incluindo arquivos ocultos e do sistema.</br>A lista a seguir descreve cada um dos valores que você pode usar para *atributos*. Usando dois-pontos (:) é opcional. Use qualquer combinação desses valores e não separe os valores com espaços.</br>diretórios **d**</br>arquivos ocultos **h**</br>**s** arquivos do sistema</br>**l** pontos de nova análise</br>arquivos somente leitura do **r**</br>**um** arquivo pronto para arquivamento</br>Não **tenho** conteúdo de arquivos indexados</br>**-** Prefixo que significa "não"|
|/o [[:] \<SortOrder >]|Classifica a saída de acordo com a *SortOrder*, que pode ser qualquer combinação dos seguintes valores:</br>**n** por nome (em ordem alfabética)</br>**e** por extensão (alfabética)</br>**g** agrupar diretórios primeiro</br>**s** por tamanho (menor primeiro)</br>**d** por data/hora (mais antigo primeiro)</br>**-** Prefixo para ordem inversa</br>Observação: O uso de dois pontos é opcional. Vários valores são processados na ordem em que são listados. Não Separe vários valores com espaços.</br>Se *SortOrder* não for especificada, **dir/o** listará os diretórios em ordem alfabética, seguidos pelos arquivos, que também são classificados em ordem alfabética.|
|/t [[:] \<TimeField >]|Especifica qual campo de hora exibir ou usar para classificação. A lista a seguir descreve cada um dos valores que você pode usar para o *timefield*:</br>criação de **c**</br>**um** último acesso</br>**w** gravado pela última vez|
|/s|Lista todas as ocorrências do nome de arquivo especificado no diretório especificado e em todos os subdiretórios.|
|/b.|Exibe uma lista simples de diretórios e arquivos, sem informações adicionais. **/b** substitui **/w**.|
|/l|Exibe nomes de diretório e nomes de arquivos não classificados em minúsculas.|
|opção|Exibe um formato de lista longa com nomes de arquivo na extrema direita da tela.|
|/x|Exibe os nomes curtos gerados para nomes de arquivo não 8dot3. A exibição é igual à exibição de **/n**, mas o nome curto é inserido antes do nome longo.|
|/c|Exibe o separador de milhar em tamanhos de arquivo. Esse é o comportamento padrão. Use **/-c** para ocultar separadores.|
|/4|Exibe anos no formato de quatro dígitos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Para usar vários parâmetros *filename* , separe cada nome de arquivo com um espaço, vírgula ou ponto-e-vírgula.
- Você pode usar caracteres curinga ( **&#42;** ou **?** ) para representar um ou mais caracteres de um nome de arquivo e exibir um subconjunto de arquivos ou subdiretórios.

  **Asterisco (\*):** Use o asterisco como um substituto para qualquer cadeia de caracteres, por exemplo:  
  - **dir \*. txt** lista todos os arquivos no diretório atual com extensões que começam com. txt, como. txt,. txt1,. txt_old.
  - **dir Read\*.txt** lista todos os arquivos no diretório atual que começam com "leitura" e com extensões que começam com. txt, como. txt,. txt1 ou. txt_old.
  - **dir Read @ no__t-1. \\** * lista todos os arquivos no diretório atual que começam com "Read" com qualquer extensão.

  O curinga asterisco sempre usa mapeamento de nome de arquivo curto, portanto, você pode obter resultados inesperados. Por exemplo, o diretório a seguir contém dois arquivos (t. txt2 e T97. txt): 
 
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

  Você pode esperar que digitar **dir T97 @ no__t-1*** retornaria o arquivo T97. txt. No entanto, a digitação de **dir T97 @ no__t-1*** retorna ambos os arquivos, pois o curinga asterisco corresponde ao arquivo t. txt2 a T97. txt usando seu mapa de nome curto T97B4 ~ 1. txt. Da mesma forma, digitar **del T97 @ no__t-1*** excluiria ambos os arquivos.

  **Ponto de interrogação (?):** Use o ponto de interrogação como um substituto para um único caractere em um nome. Por exemplo, digitando **dir de leitura???. txt** lista todos os arquivos no diretório atual com a extensão. txt que começa com "leitura" e é seguido por até três caracteres. Isso inclui Read. txt, Read1. txt, Read12. txt, Read123. txt e Readme1. txt, mas não Readme12. txt.
- Especificando atributos de exibição de arquivo

  Se você usar **/a** com mais de um valor em *atributos*, **dir** exibirá os nomes apenas desses arquivos com todos os atributos especificados. Por exemplo, se você usar **/a** com **r** e **-h** como atributos (usando **/a: r-h** ou **/ar-h**), o **dir** exibirá apenas os nomes dos arquivos somente leitura que não estão ocultos.
- Especificando a classificação de nome de arquivo

  Se você especificar mais de um valor de *SortOrder* , **dir** classificará os nomes de arquivo pelo primeiro critério e, em seguida, pelo segundo critério, e assim por diante. Por exemplo, se você usar **/o** com os valores **e** e **-s** para *SortOrder* (usando **/o: e-s** ou **/OE-s**), **dir** classificará os nomes de diretórios e arquivos por extensão, com o maior primeiro e, em seguida, exibe o resultado final. A classificação alfabética por extensão faz com que os nomes de arquivo sem extensões apareçam primeiro, depois nomes de diretório e, em seguida, nomes de arquivo com extensões.
- Usando os símbolos de redirecionamento e pipes

  Quando você usa o símbolo de redirecionamento ( **>** ) para enviar a saída **dir** para um arquivo ou um pipe ( **|** ) para enviar a saída **dir** para outro comando, use **/a:-d** e **/b** para listar apenas os nomes de arquivo. Você pode usar *nome de arquivo* com **/b** e **/s** para especificar que **dir** é Pesquisar o diretório atual e seus subdiretórios para todos os nomes de arquivo que correspondam a *filename*. **Dir** lista somente a letra da unidade, o nome do diretório, o nome do arquivo e a extensão de nome de arquivo (um caminho por linha), para cada nome de arquivo que encontrar. Antes de usar um pipe para enviar a saída **dir** para outro comando, você deve definir a variável de ambiente TEMP em seu arquivo autoexec. NT.
- O comando **dir** , com parâmetros diferentes, está disponível no console de recuperação.

## <a name="examples"></a>Exemplos

Para exibir todos os diretórios, um após o outro, em ordem alfabética, em formato amplo, e pausar após cada tela, certifique-se de que o diretório raiz é o diretório atual e, em seguida, digite:

```
dir /s/w/o/p
```

**Dir** lista o diretório raiz, os subdiretórios e os arquivos no diretório raiz, incluindo extensões. Em seguida, **dir** lista os nomes de subdiretório e nomes de arquivo em cada subdiretório na árvore.

Para alterar o exemplo anterior para que **dir** exiba os nomes de arquivo e extensões, mas omita os nomes de diretório, digite:

```
dir /s/w/o/p/a:-d
```

Para imprimir uma listagem de diretório, digite:

```
dir > prn
```

Quando você especifica **PRN**, a lista de diretórios é enviada para a impressora que está conectada à porta LPT1. Se a impressora estiver conectada a uma porta diferente, você deverá substituir **PRN** pelo nome da porta correta.

Você também pode redirecionar a saída do comando **dir** para um arquivo, substituindo **PRN** por um nome de arquivo. Você também pode digitar um caminho. Por exemplo, para direcionar a saída de **dir** para o arquivo dir. doc no diretório de registros, digite:

```
dir > \records\dir.doc
```

Se dir. doc não existir, o **dir** o criará, a menos que o diretório de registros não exista. Nesse caso, a seguinte mensagem é exibida:

`File creation error`

Para exibir uma lista de todos os nomes de arquivo com a extensão. txt em todos os diretórios na unidade C, digite:

```
dir c:\*.txt /w/o/s/p
```

O **dir** exibe, em formato largo, uma lista em ordem alfabética dos nomes de arquivo correspondentes em cada diretório e pausa sempre que a tela é preenchida até que você pressione qualquer tecla para continuar.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)