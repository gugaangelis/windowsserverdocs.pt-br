---
title: dir
description: Artigo de referência para o comando dir, que exibe uma lista de arquivos e subdiretórios de um diretório.
ms.topic: reference
ms.assetid: edcbf69b-eaa4-466e-b210-3dd8892f4d93
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c5edcbaac04d6f87721644fb4943e75456a21f66
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634996"
---
# <a name="dir"></a>dir

Exibe uma lista de arquivos e subdiretórios de um diretório. Se usado sem parâmetros, esse comando exibe o rótulo de volume e o número de série do disco, seguido por uma lista de diretórios e arquivos no disco (incluindo seus nomes e a data e hora em que cada foi modificado pela última vez). Para arquivos, esse comando exibe a extensão de nome e o tamanho em bytes. Esse comando também exibe o número total de arquivos e diretórios listados, seu tamanho cumulativo e o espaço livre (em bytes) restante no disco.

O comando **dir** também pode ser executado no console de recuperação do Windows, usando parâmetros diferentes. Para obter mais informações, consulte [ambiente de recuperação do Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxe

```
dir [<drive>:][<path>][<filename>] [...] [/p] [/q] [/w] [/d] [/a[[:]<attributes>]][/o[[:]<sortorder>]] [/t[[:]<timefield>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4] [/r]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `[<drive>:][<path>]` | Especifica a unidade e o diretório para os quais você deseja ver uma listagem. |
| `[<filename>]` | Especifica um arquivo ou grupo de arquivos específico para o qual você deseja ver uma listagem. |
| /p | Exibe uma tela da listagem de cada vez. Para ver a próxima tela, pressione qualquer tecla. |
| /q | Exibe informações de Propriedade do arquivo. |
| /w | Exibe a lista em formato largo, com até cinco nomes de arquivo ou nomes de diretório em cada linha. |
| /d | Exibe a listagem no mesmo formato que **/w**, mas os arquivos são classificados por coluna. |
| /a [[:] `<attributes>` ] | Exibe somente os nomes desses diretórios e arquivos com os atributos especificados. Se você não usar esse parâmetro, o comando exibirá os nomes de todos os arquivos, exceto arquivos ocultos e do sistema. Se você usar esse parâmetro sem especificar nenhum *atributo*, o comando exibirá os nomes de todos os arquivos, incluindo arquivos ocultos e do sistema. A lista de possíveis valores de *atributos* é:<ul><li>**d** -diretórios</li><li>**h** -arquivos ocultos</li><li>arquivos do sistema **s**</li><li>**l** -pontos de nova análise</li><li>**r** -arquivos somente leitura</li><li>**a** -arquivos prontos para arquivamento</li><li>Não há **conteúdo de arquivos** indexados</li></ul>Você pode usar qualquer combinação desses valores, mas não separe seus valores usando espaços. Opcionalmente, você pode usar dois-pontos (:) ou você pode usar um hífen (-) como um prefixo para média, "não". Por exemplo, o uso do atributo **-s** não mostrará os arquivos do sistema. |
| /o [[:] `<sortorder>` ] | Classifica a saída de acordo com a *SortOrder*, que pode ser qualquer combinação dos seguintes valores:<ul><li>**n** -alfabeticamente por nome</li><li>**e** -alfabeticamente por extensão</li><li>diretório **g** -Group First</li><li>**s** – tamanho, o menor primeiro</li><li>**d** -por data/hora, primeiro o mais antigo</li><li>Usar o **-** prefixo para reverter a ordem de classificação</li></ul>Vários valores são processados na ordem em que são listados. Não Separe vários valores com espaços, mas, opcionalmente, você pode usar dois-pontos (:).<p>Se *SortOrder* não for especificado, **dir/o** listará os diretórios em ordem alfabética, seguido pelos arquivos, que também são classificados alfabeticamente. |
| /t [[:] `<timefield>` ] | Especifica qual campo de hora exibir ou usar para classificação. Os valores de *timefield* disponíveis são:<ul><li>criação de **c**</li><li>**a** -último acessado</li><li>**w** -escrito pela última vez</li></ul> |
| /s | Lista todas as ocorrências do nome de arquivo especificado no diretório especificado e em todos os subdiretórios. |
| /b | Exibe uma lista simples de diretórios e arquivos, sem informações adicionais. O parâmetro **/b** substitui **/w**. |
| /l | Exibe nomes de diretório e nomes de arquivos não classificados, usando letras minúsculas. |
| /n | Exibe um formato de lista longa com nomes de arquivo na extrema direita da tela. |
| /x | Exibe os nomes curtos gerados para nomes de arquivo não 8dot3. A exibição é igual à exibição de **/n**, mas o nome curto é inserido antes do nome longo. |
| /c | Exibe o separador de milhar em tamanhos de arquivo. Esse é o comportamento padrão. Use **/c** para ocultar separadores. |
| /4 | Exibe anos no formato de quatro dígitos. |
| /r | Exibir fluxos de dados alternativos do arquivo. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Para usar vários parâmetros *filename* , separe cada nome de arquivo com um espaço, vírgula ou ponto-e-vírgula.

- Você pode usar caracteres curinga (**&#42;** ou **?**), para representar um ou mais caracteres de um nome de arquivo e para exibir um subconjunto de arquivos ou subdiretórios.

- Você pode usar o caractere curinga, **&#42;**, para substituir qualquer cadeia de caracteres, por exemplo:

  - `dir *.txt` lista todos os arquivos no diretório atual com extensões que começam com. txt, como. txt,. txt1,. txt_old.

  - `dir read *.txt` lista todos os arquivos no diretório atual que começam com leitura e com extensões que começam com. txt, como. txt,. txt1 ou. txt_old.

  - `dir read *.*` lista todos os arquivos no diretório atual que começam com ler com qualquer extensão.

  O curinga asterisco sempre usa mapeamento de nome de arquivo curto, portanto, você pode obter resultados inesperados. Por exemplo, o diretório a seguir contém dois arquivos (t.txt2 e t97.txt):

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

  Você pode esperar que a digitação `dir t97\*` retorne o arquivo t97.txt. No entanto, a digitação `dir t97\*` retorna os dois arquivos, porque o curinga asterisco corresponde ao arquivo t.txt2 para t97.txt usando seu mapa de nome curto *T97B4 ~1.TXT*. Da mesma forma, a digitação `del t97\*` excluiria ambos os arquivos.

- Você pode usar o ponto de interrogação (?) como um substituto para um único caractere em um nome. Por exemplo, a digitação `dir read???.txt` lista todos os arquivos no diretório atual com a extensão. txt que começam com Read e são seguidos por até três caracteres. Isso inclui Read.txt, Read1.txt, Read12.txt, Read123.txt e Readme1.txt, mas não Readme12.txt.

- Se você usar **/a** com mais de um valor em *atributos*, esse comando exibirá os nomes somente desses arquivos com todos os atributos especificados. Por exemplo, se você usar a **opção/a** com **r** e **-h** como atributos (usando `/a:r-h` ou `/ar-h` ), esse comando exibirá apenas os nomes dos arquivos somente leitura que não estão ocultos.

- Se você especificar mais de um valor de *SortOrder* , esse comando classificará os nomes de arquivo pelo primeiro critério e, em seguida, pelo segundo critério, e assim por diante. Por exemplo, se você usar **/o** com os parâmetros **e** e **-s** para *ordem* de classificação (usando `/o:e-s` ou `/oe-s` ), esse comando classificará os nomes de diretórios e arquivos por extensão, com o maior primeiro e, em seguida, exibirá o resultado final. A classificação alfabética por extensão faz com que os nomes de arquivo sem extensões apareçam primeiro, depois nomes de diretório e, em seguida, nomes de arquivo com extensões.

- Se você usar o símbolo de redirecionamento ( `>` ) para enviar a saída desse comando para um arquivo ou se usar um pipe ( `|` ) para enviar a saída desse comando para outro comando, você deverá usar `/a:-d` e **/b** para listar apenas os nomes dos arquivos. Você pode usar *nome de arquivo* com **/b** e **/s** para especificar que esse comando é para pesquisar o diretório atual e seus subdiretórios para todos os nomes de arquivo que correspondam a *filename*. Esse comando lista somente a letra da unidade, o nome do diretório, o nome do arquivo e a extensão de nome de arquivo (um caminho por linha), para cada nome de arquivo que encontrar. Antes de usar um pipe para enviar a saída desse comando para outro comando, você deve definir a variável de ambiente *Temp* no arquivo autoexec. NT.

## <a name="examples"></a>Exemplos

Para exibir todos os diretórios, um após o outro, em ordem alfabética, em formato amplo, e pausar após cada tela, certifique-se de que o diretório raiz é o diretório atual e, em seguida, digite:

```
dir /s/w/o/p
```

A saída lista o diretório raiz, os subdiretórios e os arquivos no diretório raiz, incluindo extensões. Esse comando também lista os nomes de subdiretórios e os nomes de arquivo em cada subdiretório na árvore.

Para alterar o exemplo anterior para que **dir** exiba os nomes de arquivo e extensões, mas omita os nomes de diretório, digite:

```
dir /s/w/o/p/a:-d
```

Para imprimir uma listagem de diretório, digite:

```
dir > prn
```

Quando você especifica **PRN**, a lista de diretórios é enviada para a impressora que está conectada à porta LPT1. Se a impressora estiver conectada a uma porta diferente, você deverá substituir **PRN** pelo nome da porta correta.

Você também pode redirecionar a saída do comando **dir** para um arquivo, substituindo **PRN** por um nome de arquivo. Você também pode digitar um caminho. Por exemplo, para direcionar a saída de **dir** para o arquivo dir.doc no diretório de registros, digite:

```
dir > \records\dir.doc
```

Se dir.doc não existir, **dir** o criará, a menos que o diretório de **registros** não exista. Nesse caso, a seguinte mensagem é exibida:

```
File creation error
```

Para exibir uma lista de todos os nomes de arquivo com a extensão. txt em todos os diretórios na unidade C, digite:

```
dir c:\*.txt /w/o/s/p
```

O comando **dir** exibe, em formato amplo, uma lista em ordem alfabética dos nomes de arquivo correspondentes em cada diretório e pausa cada vez que a tela é preenchida até que você pressione qualquer tecla para continuar.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
