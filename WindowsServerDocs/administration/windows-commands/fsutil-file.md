---
ms.assetid: 9f3dc104-dd69-4b03-b824-a29896780164
title: Arquivo fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 236f951286e2ed2e16a329a04e912b812d020aa6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725509"
---
# <a name="fsutil-file"></a>Arquivo fsutil
> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Localiza um arquivo por nome de usuário (se as cotas de disco estiverem habilitadas), consulta intervalos alocados para um arquivo, define o nome curto de um arquivo, define o comprimento de dados válido de um arquivo, define zero data para um arquivo ou cria um novo arquivo.



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

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|CreateNew|Cria um arquivo com o nome e o tamanho especificados, com conteúdo que consiste em zeros.|
|\<nome de arquivo>|Especifica o caminho completo para o arquivo, incluindo o nome do arquivo e a extensão, por exemplo, C:\documents\filename.txt.|
|\<comprimento>|Especifica o comprimento de dados válido do arquivo.|
|findbysid|Localiza os arquivos que pertencem a um usuário especificado em volumes NTFS em que as cotas de disco estão habilitadas.|
|\<nome de usuário>|Especifica o nome de usuário ou o nome de logon do usuário.|
|\<> de diretório|Especifica o caminho completo para o diretório, por exemplo C:\Users.|
|optimizemetadata|Isso executa uma compactação imediata dos metadados para um determinado arquivo.|
|/A|Analise os metadados do arquivo antes e depois da otimização.|
|queryallocranges|Consulta os intervalos alocados para um arquivo em um volume NTFS. Útil para determinar se um arquivo tem regiões esparsas.|
|offset =\<deslocamento>|Especifica o início do intervalo que deve ser definido como zeros.|
|comprimento =\<comprimento>|Especifica o comprimento do intervalo (em bytes).|
|queryextents|As extensões de consultas para um arquivo.|
|/R|Se <filename> for um ponto de nova análise, abra-o em vez de seu destino.|
|\<> startingvcn|Especifica primeiro VCN a ser consultado. Se omitido, inicie em VCN 0.|
|\<> numvcns|Número de VCNs a ser consultado. Se for omitido ou 0, consulte até EOF.|
|queryfileid|Consulta a ID de arquivo de um arquivo em um volume NTFS.<p>Esse parâmetro aplica-se a: Windows Server 2008 R2 e Windows 7.|
|\<> de volume|Especifica o volume como nome da unidade seguido por dois-pontos.|
|queryfilenamebyid|Exibe um nome de link aleatório para uma ID de arquivo especificada em um volume NTFS. Como um arquivo pode ter mais de um nome de link apontando para esse arquivo, não há garantia de que o link de arquivo será fornecido como resultado da consulta para o nome do arquivo.<p>Esse parâmetro aplica-se a: Windows Server 2008 R2 e Windows 7.|
|\<> FileID|Especifica a ID do arquivo em um volume NTFS.|
|queryoptimizemetadata|Consulta o estado de metadados de um arquivo.|
|queryvaliddata|Consulta o comprimento de dados válido para um arquivo.|
|/D|Exibir informações de dados válidas detalhadas.|
|seteof|Define o EOF do arquivo fornecido.|
|setcurtoname|Define o nome curto (8,3 nome de arquivo de comprimento de caractere) para um arquivo em um volume NTFS.|
|\<> de curtoname|Especifica o nome curto do arquivo.|
|setvaliddata|Define o comprimento de dados válido para um arquivo em um volume NTFS.|
|\<> de comprimento|Especifica o comprimento do arquivo em bytes.|
|setzerodata|Define um intervalo (especificado por *deslocamento* e *comprimento*) do arquivo como zeros, o que esvazia o arquivo. Se o arquivo for um arquivo esparso, as unidades de alocação subjacentes serão confirmadas.|

## <a name="remarks"></a>Comentários

-   No NTFS, há dois conceitos importantes de comprimento de arquivo: o marcador de fim de arquivo (EOF) e o comprimento de dados válido (VDL). O EOF indica o comprimento real do arquivo. O VDL identifica o comprimento de dados válidos em disco. Todas as leituras entre VDL e EOF retornam automaticamente 0 para preservar o requisito de reutilização do objeto C2.

-   O parâmetro **setvaliddata** só está disponível para administradores porque requer o privilégio executar tarefas de manutenção de volume (SeManageVolumePrivilege). Esse recurso só é necessário para cenários avançados de rede de área de sistema e multimídia. O parâmetro **setvaliddata** deve ser um valor positivo maior que o VDL atual, mas menor que o tamanho do arquivo atual.

    É útil para os programas definir um VDL quando:

    -   Gravando clusters brutos diretamente no disco por meio de um canal de hardware. Isso permite que o programa Informe ao sistema de arquivos que esse intervalo contém dados válidos que podem ser retornados ao usuário.

    -   Criando arquivos grandes quando o desempenho é um problema. Isso evita o tempo necessário para preencher o arquivo com zeros quando o arquivo é criado ou estendido.

## <a name="examples"></a><a name="BKMK_examples"></a>Disso
Para localizar arquivos que são de propriedade de scottb na unidade C, digite:

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

Para consultar as extensões de um arquivo, digite:

```
fsutil file queryextents C:\Temp\sample.txt
```

Para definir o EOF de um arquivo, digite:

```
fsutil file seteof C:\testfile.txt 1000
```

Para definir o nome curto do arquivo LongFileName. txt na unidade C como Longfile. txt, digite:

```
fsutil file setshortname c:\longfilename.txt longfile.txt  
```

Para definir o comprimento de dados válido para 4096 bytes para um arquivo chamado TestFile. txt em um volume NTFS, digite:

```
fsutil file setvaliddata c:\testfile.txt 4096  
```

Para definir um intervalo de um arquivo em um volume NTFS como zeros para esvaziá-lo, digite:

```
fsutil file setzerodata offset=100 length=150 c:\temp\sample.txt  
```

## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


