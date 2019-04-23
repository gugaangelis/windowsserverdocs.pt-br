---
ms.assetid: 9f3dc104-dd69-4b03-b824-a29896780164
title: Arquivo do fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: ffaf02f74f20f4eb94b94d8f0ffc51f26a62390e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828117"
---
# <a name="fsutil-file"></a>Arquivo do fsutil
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Localiza um arquivo por nome de usuário (se as cotas de disco estiverem habilitadas), consulta intervalos alocados para um arquivo, define o nome curto de um arquivo, define o comprimento de dados válido do arquivo, define zero dados para um arquivo ou cria um novo arquivo.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fsutil file [createnew] <filename> <length>
fsutil file [findbysid] <username> <directory>
fsutil file [optimizemetadata] [/A] <filename>
fsutil file [queryallocranges] offset=<offset> length=<length> <filename>
fsutil file [queryextents] [/R] <filename> [<startingvcn> [<numvcns>]]
fsutil file [queryfileid] <filename>
fsutil file [queryfilenamebyid] <volume> <fileid>
fsutil file [queryoptimizemetadata] <filename>
fsutil file [queryvaliddata] [/R] [/D] <filename>
fsutil file [seteof] <filename> <length>
fsutil file [setshortname] <filename> <shortname>
fsutil file [setvaliddata] <filename> <datalength>
fsutil file [setzerodata] offset=<offset> length=<length> <filename>

```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|CreateNew|Cria um arquivo do nome especificado e o tamanho, com conteúdo que consiste em zeros.|
|\<filename>|Especifica o caminho completo para o arquivo, incluindo o nome de arquivo e extensão, por exemplo C:\documents\filename.txt.|
|\<length>|Especifica o comprimento de dados válido do arquivo.|
|findbysid|Localiza arquivos que pertencem a um usuário especificado em volumes NTFS em que as cotas de disco estão habilitadas.|
|\<username>|Especifica o nome de usuário ou o nome de logon do usuário.|
|\<directory>|Especifica o caminho completo para o diretório, por exemplo, C:\users.|
|optimizemetadata|Isso executa uma compactação imediata dos metadados para um determinado arquivo.|
|/A|Analise os metadados do arquivo antes e depois da otimização.|
|queryallocranges|Pesquisa os intervalos alocados para um arquivo em um volume NTFS. É útil para determinar se um arquivo possui regiões esparsas.|
|offset=\<offset>|Especifica o início do intervalo que deve ser definido como zeros.|
|length=\<length>|Especifica o comprimento do intervalo (em bytes).|
|queryextents|Extensões de consultas para um arquivo.|
|/R|Se <filename> é uma nova análise ponto, abri-lo em vez de seu destino.|
|\<startingvcn>|Especifica o primeiro VCN à consulta. Se omitido, começam em VCN 0.|
|\<numvcns>|Número de VCNs para consultar. Se omitido ou 0, a consulta até o EOF.|
|queryfileid|Consulta a ID do arquivo de um arquivo em um volume NTFS.<br /><br />Esse parâmetro se aplica a:  Windows Server 2008 R2 e Windows 7.|
|\<volume>|Especifica o volume como nome de unidade seguido por dois-pontos.|
|queryfilenamebyid|Exibe um nome de link aleatória para uma ID de arquivo especificado em um volume NTFS. Como um arquivo pode ter mais de um nome de link que aponta para esse arquivo, não há garantia de qual link de arquivo será fornecido em virtude da consulta para o nome do arquivo.<br /><br />Esse parâmetro se aplica a:  Windows Server 2008 R2 e Windows 7.|
|\<fileid>|Especifica a ID do arquivo em um volume NTFS.|
|queryoptimizemetadata|Consulta o estado de metadados de um arquivo.|
|queryvaliddata|Consulta o comprimento de dados válido para um arquivo.|
|/D|Exiba informações detalhadas de dados válido.|
|seteof|Define o EOF de determinado arquivo.|
|setshortname|Define o nome curto (nome de arquivo 8.3) para um arquivo em um volume NTFS.|
|\<shortname>|Especifica o nome curto de arquivo.|
|setvaliddata|Define o comprimento de dados válido para um arquivo em um volume NTFS.|
|\<DataLength >|Especifica o tamanho do arquivo em bytes.|
|setzerodata|Define um intervalo (especificado por *deslocamento* e *comprimento*) do arquivo em zeros, que esvazia o arquivo. Se o arquivo é um arquivo esparso, as unidades de alocação subjacentes são não comprometidas.|

## <a name="remarks"></a>Comentários

-   No NTFS, há dois conceitos importantes de tamanho de arquivo: o marcador de fim-de-arquivo (EOF) e o comprimento de dados válido (VDL). O EOF indica o comprimento real do arquivo. O VDL identifica o comprimento de dados válidos no disco. Qualquer leitura entre VDL e EOF retorna automaticamente o requisito de reutilização de 0 para preservar o objeto C2.

-   O **setvaliddata** parâmetro só está disponível para administradores, porque ele requer o privilégio de tarefas (SeManageVolumePrivilege) de manutenção de volume de executar. Esse recurso é necessário apenas para cenários de rede avançados de multimídia e sistema de área. O **setvaliddata** parâmetro deve ser um valor positivo que seja maior que o VDL atual, mas menor do que o tamanho do arquivo atual.

    É útil para programas para definir um VDL quando:

    -   Gravar clusters não processados diretamente no disco por meio de um canal de hardware. Isso permite que o programa informar ao sistema de arquivos que esse intervalo contiver dados válidos que podem ser retornados ao usuário.

    -   Criar arquivos grandes, quando o desempenho é um problema. Isso evita o tempo necessário para preencher o arquivo com zeros quando o arquivo é criado ou estendido.

## <a name="BKMK_examples"></a>Exemplos
Para localizar arquivos que são ederp na unidade C, digite:

```
fsutil file findbysid scottb c:\users  
```

Para consultar os intervalos alocados para um arquivo em um volume NTFS, digite:

```
fsutil file queryallocranges offset=1024 length=64 c:\temp\sample.txt  
```

Para otimizar os metadados de um arquivo, digite:

```
fsutil file optimizemetadata C:\largefragmentedfile.txt
```

Para consultar as extensões para um arquivo, digite:

```
fsutil file queryextents C:\Temp\sample.txt
```

Para definir o EOF para um arquivo, digite:

```
fsutil file seteof C:\testfile.txt 1000
```

Para definir o nome curto para o arquivo nomearquivolongo na unidade C como arqlongo. txt, digite:

```
fsutil file setshortname c:\longfilename.txt longfile.txt  
```

Para definir o comprimento de dados válido de 4096 bytes para um arquivo denominado Testfile. txt em um volume NTFS, digite:

```
fsutil file setvaliddata c:\testfile.txt 4096  
```

Para definir um intervalo de um arquivo em um volume NTFS em zeros para esvaziá-lo, digite:

```
fsutil file setzerodata offset=100 length=150 c:\temp\sample.txt  
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


