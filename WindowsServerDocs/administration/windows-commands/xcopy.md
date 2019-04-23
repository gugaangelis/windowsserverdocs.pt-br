---
title: xcopy
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76a310d7-9925-4571-a252-0e28960d5f89
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 01/05/2019
ms.openlocfilehash: 54697b1c967d3e21583977418383d5a372e6f5d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859397"
---
# <a name="xcopy"></a>xcopy



Copia os arquivos e pastas, inclusive subpastas

Para obter exemplos de como usar esse comando, consulte [exemplos](xcopy.md#BKMK_examples)

## <a name="syntax"></a>Sintaxe

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<origem >|Obrigatório. Especifica o local e os nomes dos arquivos que você deseja copiar. Esse parâmetro deve incluir uma unidade ou um caminho.|
|[\<Destination>]|Especifica o destino dos arquivos que você deseja copiar. Esse parâmetro pode incluir uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses elementos.|
|/w|Exibe a seguinte mensagem e aguarda sua resposta antes de começar a copiar arquivos:</br>**Pressione qualquer tecla para iniciar a cópia de arquivo (s)**|
|/p|Solicita que você confirme se você deseja criar cada arquivo de destino.|
|/c|Ignora erros.|
|/v|Verifica cada arquivo conforme ele é gravado no arquivo de destino para certificar-se de que os arquivos de destino são idênticos aos arquivos de origem.|
|/q|Suprime a exibição de **xcopy** mensagens.|
|/f|Exibe os nomes de arquivo de origem e de destino durante a cópia.|
|/l|Exibe uma lista de arquivos que devem ser copiados.|
|/g|Cria descriptografado *destino* arquivos quando o destino não der suporte à criptografia.|
|/d [: dd-mm-aaaa]|Cópias de arquivos de origem alterados em ou após a data especificada. Se você não incluir um *dd-mm-aaaa* valor, **xcopy** copia todos os *origem* arquivos que são mais recentes que existente *destino* arquivos. Essa opção de linha de comando permite que você atualize os arquivos que foram alterados.|
|/u|Copia os arquivos de *fonte* que existem na *destino* apenas.|
|/i|Se *fonte* é um diretório ou contém caracteres curinga e *destino* não existe **xcopy** pressupõe *destino* Especifica um nome do diretório e cria um novo diretório. Em seguida, **xcopy** copia todos os arquivos especificados para o novo diretório. Por padrão, **xcopy** solicita que você especifique se *destino* é um arquivo ou diretório.|
|/s|Copia os diretórios e subdiretórios, a menos que elas estiverem vazias. Se você omitir **/s**, **xcopy** funciona dentro de um único diretório.|
|/e|Copia todos os subdiretórios, mesmo se elas estiverem vazias. Use **/e** com o **/s** e **/t** opções de linha de comando.|
|/t|Copia que a estrutura de subpasta (ou seja, a árvore) apenas, não os arquivos. Para copiar pastas vazias, você deve incluir a **/e** opção de linha de comando.|
|/k|Copia os arquivos e mantém o atributo somente leitura nos *destino* arquivos, se presente na *origem* arquivos. Por padrão, **xcopy** remove o atributo somente leitura.|
|/r|Copia arquivos somente leitura.|
|/h|Copia os arquivos com ocultos e os atributos de arquivo do sistema. Por padrão, **xcopy** não não cópia ocultada ou arquivos do sistema|
|/a|Copia só *origem* arquivos que têm seu arquivamento de atributos de arquivo. **/a** não modifica o atributo de arquivo do arquivo de origem. Para obter informações sobre como definir o atributo de arquivo usando **attrib**, consulte [referências adicionais](xcopy.md#BKMK_addref).|
|/m|Cópias *origem* arquivos que têm seu arquivamento de atributos de arquivo. Diferentemente **/a**, **/m** desativa o atributo de arquivo nos arquivos que são especificados na origem. Para obter informações sobre como definir o atributo de arquivo usando **attrib**, consulte [referências adicionais](xcopy.md#BKMK_addref).|
|/n|Cria cópias usando o curto de arquivo NTFS ou nomes de diretório. **/n** é necessário quando você copiar arquivos ou diretórios de um volume NTFS para um volume FAT ou quando o FAT arquivo convenção de nomenclatura do sistema (ou seja, 8.3 caracteres) é necessário em de *destino* sistema de arquivos. O *destino* sistema de arquivos pode ser FAT ou NTFS.|
|/o|Cópias de propriedade de arquivos e informações de DACL (lista) de controle de acesso condicional.|
|/x|Copia as configurações de auditoria e informações de SACL (lista) de controle de acesso do sistema de arquivos (implica **/o**).|
|/exclude:FileName1[+[FileName2][+[FileName3]( \)]|Especifica uma lista de arquivos. Pelo menos um arquivo deve ser especificado. Cada arquivo contém cadeias de caracteres de pesquisa com cada cadeia de caracteres em uma linha separada no arquivo.</br>Quando qualquer uma das cadeias de caracteres corresponde a qualquer parte do caminho absoluto do arquivo a ser copiado, esse arquivo será excuded que está sendo copiado. Por exemplo, especificar a cadeia de caracteres **obj** excluirá todos os arquivos sob o diretório **obj** ou todos os arquivos com o **obj** extensão.|
|/y|Suprime a solicitação para confirmar que você deseja substituir um arquivo de destino existente.|
|/-y|Os prompts para confirmar que você deseja substituir um arquivo de destino existente.|
|/z|Copia em uma rede no modo reiniciável.|
|/b|Copia o link simbólico em vez dos arquivos. Esse parâmetro foi introduzido no Windows Vista®.|
|/j|Copia os arquivos sem buffer. Recomendado para arquivos muito grandes. Esse parâmetro foi adicionado no Windows Server 2008 R2.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Usando **/z.**

    Se você perder sua conexão durante a fase de cópia (por exemplo, se o servidor ficar offline rompe a conexão), ela será reiniciada após você restabelecer a conexão. **/z** também exibe a porcentagem da operação de cópia concluída para cada arquivo.
-   Usando o **/y** na variável de ambiente COPYCMD.

    Você pode usar **/y** na variável de ambiente COPYCMD. Você pode substituir esse comando, usando **y/** na linha de comando. Por padrão, você é solicitado a substituir.
-   Copiando arquivos criptografados

    Copiando arquivos criptografados para um volume que não oferece suporte a EFS resulta em um erro. Descriptografar os arquivos pela primeira vez ou copiar os arquivos para um volume que dá suporte a EFS.
-   Anexando arquivos

    Para acrescentar arquivos, especifique um único arquivo de destino, mas vários arquivos de origem (ou seja, usando caracteres curinga ou arquivo1 + arquivo2 + arquivo3 de formato).
-   Valor padrão para *destino*

    Se você omitir *destino*, o **xcopy** comando copia os arquivos no diretório atual.
-   Especificando se *destino* é um arquivo ou pasta

    Se *destino* não contém um diretório existente e não termina com uma barra invertida (\), a seguinte mensagem aparecerá:  
    ```
    Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
    ```  
    Se você quiser que o arquivo ou arquivos a serem copiados para um arquivo, pressione F. Pressione D se desejar que o arquivo ou arquivos a serem copiados para um diretório.

    Você pode suprimir esta mensagem usando o **/i** opção de linha de comando, que faz com que **xcopy** pressupor que o destino é um diretório, se a fonte for mais de um arquivo ou diretório.
-   Usando o **xcopy** comando para definir o atributo archive para *destino* arquivos

    O **xcopy** comando cria arquivos com o conjunto de atributos de arquivo morto, se esse atributo foi definido no arquivo de origem. Para obter mais informações sobre atributos de arquivo e **attrib**, consulte [referências adicionais](xcopy.md#BKMK_addref).
-   Comparando **xcopy** e **diskcopy**

    Se você tiver um disco que contenha arquivos em subpastas e quiser copiá-lo para um disco que tem um formato diferente, use o **xcopy** comando em vez de **diskcopy**. Porque o **diskcopy** comando copia discos trilha por trilha, seus discos de origem e de destino devem ter o mesmo formato. O **xcopy** comando não tem esse requisito. Use **xcopy** , a menos que você precisa de uma cópia da imagem completa do disco.
-   Códigos de saída de **xcopy**

    Para processar códigos de saída retornados por **xcopy**, use o **ErrorLevel** parâmetro no **se** linha de comando em um arquivo em lotes. Para obter um exemplo de um arquivo em lotes que processe usando os códigos de saída **se**, consulte [referências adicionais](xcopy.md#BKMK_addref). A tabela a seguir lista cada código de saída e uma descrição.  
    |Código de Saída|Descrição|
    |---------|-----------|
    |0|Os arquivos foram copiados sem erro.|
    |1|Não foram encontrados arquivos para copiar.|
    |2|O usuário pressionou CTRL + C para encerrar **xcopy**.|
    |4|Erro de inicialização. Não há suficiente memória ou espaço em disco, ou você inseriu um nome de unidade inválido ou uma sintaxe inválida na linha de comando.|
    |5|Ocorreu erro de gravação de disco.|

## <a name="BKMK_examples"></a>Exemplos

**1.** Para copiar todos os arquivos e subdiretórios (incluindo todos os subdiretórios vazios) da unidade A para a unidade B, digite:
```
xcopy a: b: /s /e 
```
**2.** Para incluir qualquer sistema ou arquivos ocultos no exemplo anterior, adicione a **/h** a opção de linha de comando da seguinte maneira:
```
xcopy a: b: /s /e /h
```
**3.** Para atualizar os arquivos no diretório \Reports com os arquivos no diretório \Rawdata que foram alterados desde o dia 29 de dezembro de 1993, digite:
```
xcopy \rawdata \reports /d:12-29-1993
```
**4.** Para atualizar todos os arquivos que existem em \Reports no exemplo anterior, independentemente da data, digite:
```
xcopy \rawdata \reports /u
```
**5.** Para obter uma lista dos arquivos a serem copiados pelo comando anterior (ou seja, sem realmente copiar os arquivos), digite:
```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```
O arquivo xcopy lista todos os arquivos serão copiados.

**6.** Para copiar o diretório \Customer e todos os subdiretórios no diretório \\ \\Public\Address na rede unidade h:, manter o atributo somente leitura e sejam solicitados quando um novo arquivo for criado em h:, digite:
```
xcopy \customer h:\public\address /s /e /k /p
```
**7.** Para emitir o comando anterior, certifique-se de que **xcopy** cria o diretório de \Address se ele não existir e suprimir a mensagem que aparece quando você cria um novo diretório, adicione o **/i** de linha de comando opção da seguinte maneira:
```
xcopy \customer h:\public\address /s /e /k /p /i
```
**8.** Você pode criar um arquivo em lotes para realizar **xcopy** operações e use o lote **se** comando para processar o código de saída, se ocorrer um erro. Por exemplo, o programa de lote a seguir usa parâmetros substituíveis para o **xcopy** parâmetros de origem e de destino:
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
Para usar o programa de lote anterior para copiar todos os arquivos no diretório C:\Prgmcode e seus subdiretórios para a unidade B, digite:
```
copyit c:\prgmcode b:
```
Substitutos de interpretador de comando **C:\Prgmcode** para *%1* e **b:** para *%2*, em seguida, usa **xcopy**com o **/e** e **/s** opções de linha de comando. Se **xcopy** encontrar um erro, o arquivo em lotes lê o código de saída e vai para o rótulo indicado no respectivo **IF ERRORLEVEL** instrução, em seguida, exibe a mensagem apropriada e sairá das arquivo em lotes.

**9.** Neste exemplo, todos o não-vazia diretórios, além de arquivos cujo nome corresponde ao padrão fornecido com o símbolo de asterisco.
```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```
No exemplo anterior, esse valor de parâmetro de origem em particular **.\\ sumário\*. yml** copiar o igual 3 arquivos, mesmo que seus caracteres de dois caminho **.\\**  foram removidos. No entanto, não há arquivos seriam copiados se o caractere curinga asterisco foi removido do parâmetro de origem, tornando-o apenas **.\\ TOC.yml**.

#### <a name="BKMK_addref"></a>Referências adicionais

-   [Cópia](copy.md)
-   [Mover](move.md)
-   [Dir](dir.md)
-   [Attrib](attrib.md)
-   [Diskcopy](diskcopy.md)
-   [If](if.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
