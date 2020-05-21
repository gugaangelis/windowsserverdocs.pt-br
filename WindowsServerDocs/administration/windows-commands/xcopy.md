---
title: xcopy
description: Tópico de referência para xcopy, que copia arquivos e diretórios, incluindo subdiretórios.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 76a310d7-9925-4571-a252-0e28960d5f89
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 01/05/2019
ms.openlocfilehash: 0145de2828c1d33cf1b82f595dd6c00812ace54e
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436781"
---
# <a name="xcopy"></a>xcopy

Copia arquivos e diretórios, incluindo subdiretórios.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#examples).

## <a name="syntax"></a>Sintaxe

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> de origem|Obrigatórios. Especifica o local e os nomes dos arquivos que você deseja copiar. Esse parâmetro deve incluir uma unidade ou um caminho.|
|[ \< Destino>]|Especifica o destino dos arquivos que você deseja copiar. Esse parâmetro pode incluir uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses.|
|/w|Exibe a mensagem a seguir e aguarda sua resposta antes de começar a copiar arquivos:</br>**Pressione qualquer tecla para começar a Copiar arquivo (s)**|
|/p|Solicita que você confirme se deseja criar cada arquivo de destino.|
|/c|Ignora erros.|
|/v|Verifica cada arquivo conforme ele é gravado no arquivo de destino para garantir que os arquivos de destino sejam idênticos aos arquivos de origem.|
|/q|Suprime a exibição de mensagens do **xcopy** .|
|/f|Exibe os nomes de arquivos de origem e de destino ao copiar.|
|/l|Exibe uma lista de arquivos que devem ser copiados.|
|/g|Cria arquivos de *destino* descriptografados quando o destino não dá suporte à criptografia.|
|/d [: MM-DD-AAAA]|Copia os arquivos de origem alterados em ou após a data especificada apenas. Se você não incluir um valor *mm-dd-aaaa* , o **xcopy** copiará todos os arquivos de *origem* mais recentes do que os arquivos de *destino* existentes. Essa opção de linha de comando permite que você atualize os arquivos que foram alterados.|
|/u|Copia arquivos da *origem* que existem somente no *destino* .|
|/i|Se a *origem* for um diretório ou contiver curingas e o *destino* não existir, **xcopy** pressupõe que o *destino* especifique um nome de diretório e crie um novo diretório. Em seguida, **xcopy** copia todos os arquivos especificados para o novo diretório. Por padrão, o **xcopy** solicita que você especifique se o *destino* é um arquivo ou um diretório.|
|/s|Copia diretórios e subdiretórios, a menos que estejam vazios. Se você omitir **/s**, o **xcopy** funcionará em um único diretório.|
|/e|Copia todos os subdiretórios, mesmo que estejam vazios. Use **/e** com as opções de linha de comando **/s** e **/t** .|
|/t|Copia a estrutura de subdiretório (ou seja, a árvore) somente, não arquivos. Para copiar diretórios vazios, você deve incluir a opção de linha de comando **/e** .|
|/k|Copia arquivos e retém o atributo somente leitura nos arquivos de *destino* , se estiverem presentes nos arquivos de *origem* . Por padrão, o **xcopy** remove o atributo somente leitura.|
|/r|Copia arquivos somente leitura.|
|/h|Copia arquivos com atributos de arquivo ocultos e de sistema. Por padrão, o **xcopy** não copia arquivos ocultos ou do sistema|
|/a|Copia somente os arquivos de *origem* que têm seus atributos de arquivo morto definidos. **/a** não modifica o atributo de arquivo morto do arquivo de origem. Para obter informações sobre como definir o atributo arquivo morto usando **attrib**, consulte [referências adicionais](#additional-references).|
|/m|Copia os arquivos de *origem* que têm seus atributos de arquivo morto definidos. Ao contrário de **/a**, **/m** desativa os atributos do arquivo morto nos arquivos especificados na origem. Para obter informações sobre como definir o atributo arquivo morto usando **attrib**, consulte [referências adicionais](#additional-references).|
|/n|Cria cópias usando o arquivo curto NTFS ou nomes de diretório. **/n** é necessário quando você copia arquivos ou diretórios de um volume NTFS para um volume FAT ou quando a Convenção de nomenclatura do sistema de arquivos FAT (ou seja, 8,3 caracteres) é necessária no sistema de arquivos de *destino* . O sistema de arquivos de *destino* pode ser FAT ou NTFS.|
|/o|Copia a propriedade do arquivo e as informações de DACL (lista de controle de acesso discricionário).|
|/x|Copia as configurações de auditoria de arquivo e as informações de SACL (lista de controle de acesso do sistema) (implicam **/o**).|
|/exclude: arquivo1 [+ [Nome_de_arquivo2] [+ [FileName3] ( \) ]|Especifica uma lista de arquivos. Pelo menos um arquivo deve ser especificado. Cada arquivo conterá cadeias de pesquisa com cada cadeia de caracteres em uma linha separada no arquivo.</br>Quando qualquer uma das cadeias de caracteres corresponder a qualquer parte do caminho absoluto do arquivo a ser copiado, esse arquivo será excluído da cópia. Por exemplo, especificar a cadeia de caracteres **obj** excluirá todos os arquivos abaixo do diretório **obj** ou todos os arquivos com a extensão **. obj** .|
|/y|Suprime a solicitação para confirmar que você deseja substituir um arquivo de destino existente.|
|/-y|Solicita que você confirme se deseja substituir um arquivo de destino existente.|
|/z|Copia em uma rede no modo reiniciável.|
|/b|Copia o link simbólico em vez dos arquivos. Esse parâmetro foi introduzido no Windows Vista®.|
|/j|Copia arquivos sem buffer. Recomendado para arquivos muito grandes. Esse parâmetro foi adicionado no Windows Server 2008 R2.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Usando **/z**

  Se você perder a conexão durante a fase de cópia (por exemplo, se o servidor que está ficando offline for a conexão), ele será retomado depois que você restabelecer a conexão. **/z** também exibe a porcentagem da operação de cópia concluída para cada arquivo.

- Usando **/y** na variável de ambiente COPYCMD.

  Você pode usar **/y** na variável de ambiente COPYCMD. Você pode substituir esse comando usando **/-y** na linha de comando. Por padrão, você será solicitado a substituir.

- Copiando arquivos criptografados

  A cópia de arquivos criptografados para um volume que não dá suporte ao EFS resulta em um erro. Descriptografe os arquivos primeiro ou copie os arquivos para um volume que ofereça suporte ao EFS.

- Anexando arquivos

  Para acrescentar arquivos, especifique um único arquivo para o destino, mas vários arquivos de origem (ou seja, usando curingas ou o formato arquivo1 + arquivo2 + arquivo3).

- Valor padrão para *destino*

  Se você omitir o *destino*, o comando **xcopy** copiará os arquivos para o diretório atual.

- Especificando se o *destino* é um arquivo ou diretório

  Se o *destino* não contiver um diretório existente e não terminar com uma barra invertida ( \) , a seguinte mensagem será exibida:

  ```
  Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
  ```

Pressione F se desejar que o arquivo ou os arquivos sejam copiados para um arquivo. Pressione D se desejar que o arquivo ou os arquivos sejam copiados para um diretório.

  Você pode suprimir essa mensagem usando a opção de linha de comando **/i** , que faz com que **xcopy** assuma que o destino é um diretório se a origem for mais de um arquivo ou um diretório.
- Usando o comando **xcopy** para definir o atributo de arquivo morto para arquivos de *destino*

  O comando **xcopy** cria arquivos com o atributo de arquivo definido, independentemente de esse atributo ter sido definido ou não no arquivo de origem. Para obter mais informações sobre atributos de arquivo e **Atrib**, consulte [referências adicionais](#additional-references).

- Comparando **xcopy** e **diskcopy**

  Se você tiver um disco que contém arquivos em subdiretórios e quiser copiá-lo para um disco que tenha um formato diferente, use o comando **xcopy** em vez de **diskcopy**. Como o comando **diskcopy** copia os discos controlados por faixa, os discos de origem e de destino devem ter o mesmo formato. O comando **xcopy** não tem esse requisito. Use **xcopy** , a menos que você precise de uma cópia de imagem de disco completa.

- Códigos de saída para **xcopy**

  Para processar códigos de saída retornados pelo **xcopy**, use o parâmetro **ERRORLEVEL** na linha de comando **If** em um programa em lotes. Para obter um exemplo de um programa em lotes que processa códigos de saída usando **If**, consulte [referências adicionais](#additional-references). A tabela a seguir lista cada código de saída e uma descrição.

  |Código de saída|Descrição|
  |---------|-----------|
  |0|Os arquivos foram copiados sem erros.|
  |1|Nenhum arquivo foi encontrado para cópia.|
  |2|O usuário pressionou CTRL + C para encerrar o **xcopy**.|
  |4|Ocorreu um erro de inicialização. Não há memória ou espaço em disco suficiente ou você inseriu um nome de unidade inválido ou uma sintaxe inválida na linha de comando.|
  |5|Erro de gravação no disco.|

## <a name="examples"></a>Exemplos

**1.** para copiar todos os arquivos e subdiretórios (incluindo todos os subdiretórios vazios) da unidade a para a unidade B, digite:

```
xcopy a: b: /s /e
```

**2.** para incluir qualquer sistema ou arquivo oculto no exemplo anterior, adicione a opção de linha de comando<strong>/h</strong> da seguinte maneira:

```
xcopy a: b: /s /e /h
```

**3.** para atualizar os arquivos no diretório \Reports com os arquivos no diretório \Rawdata que foram alterados desde 29 de dezembro de 1993, digite:

```
xcopy \rawdata \reports /d:12-29-1993
```

**4.** para atualizar todos os arquivos existentes no \Reports no exemplo anterior, independentemente da data, digite:

```
xcopy \rawdata \reports /u
```

**5.** para obter uma lista dos arquivos a serem copiados pelo comando anterior (ou seja, sem realmente copiar os arquivos), digite:

```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```

O arquivo Xcopy. out lista todos os arquivos que serão copiados.

**6.** para copiar o diretório \Customer e todos os subdiretórios para o diretório \\ \\ Public\Address na unidade de rede H:, mantenha o atributo somente leitura e seja solicitado quando um novo arquivo for criado em H:, digite:

```
xcopy \customer h:\public\address /s /e /k /p
```

**7.** para emitir o comando anterior, certifique-se de que **xcopy** cria o diretório \Address se ele não existir e suprimir a mensagem que aparece quando você cria um novo diretório, adicione a opção de linha de comando **/i** da seguinte maneira:

```
xcopy \customer h:\public\address /s /e /k /p /i
```

**8.** você pode criar um programa em lotes para executar operações de **xcopy** e usar o comando Batch **If** para processar o código de saída se ocorrer um erro. Por exemplo, o seguinte programa em lotes usa parâmetros substituíveis para os parâmetros de origem e destino do **xcopy** :

```
@echo off
rem COPYIT.BAT transfers all files in all subdirectories of
rem the source drive or directory (%1) to the destination
rem drive or directory (%2)
xcopy %1 %2 /s /e
if errorlevel 4 goto lowmemory
if errorlevel 2 goto abort
if errorlevel 0 goto exit
:lowmemory
echo Insufficient memory to copy files or
echo invalid drive or command-line syntax.
goto exit
:abort
echo You pressed CTRL+C to end the copy operation.
goto exit
:exit
```

Para usar o programa em lotes anterior para copiar todos os arquivos no diretório C:\Prgmcode e seus subdiretórios para a unidade B, digite:

```
copyit c:\prgmcode b:
```

O interpretador de comando substitui **C:\Prgmcode** para *%1* e **B:** para *%2*, usa **xcopy** com as opções de linha de comando **/e** e **/s** . Se **xcopy** encontrar um erro, o programa em lotes lerá o código de saída e vai para o rótulo indicado na instrução **If ERRORLEVEL** apropriada e, em seguida, exibirá a mensagem apropriada e sairá do programa em lotes.

**9.** este exemplo todos os diretórios não vazios, além de arquivos cujo nome corresponde ao padrão fornecido com o símbolo de asterisco.

```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```

No exemplo anterior, esse valor de parâmetro de origem específico **. \\ TOC \* . yml** copiará os mesmos três arquivos mesmo que seus dois caracteres de caminho **. \\ ** tenham sido removidos. No entanto, nenhum arquivo seria copiado se o curinga de asterisco fosse removido do parâmetro de origem, tornando-o apenas **. \\ TOC. yml**.

## <a name="additional-references"></a>Referências adicionais

-   [Cópia](copy.md)
-   [Mover](move.md)
-   [Comando](dir.md)
-   [Atributos](attrib.md)
-   [Diskcopy](diskcopy.md)
-   [Que](if.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
