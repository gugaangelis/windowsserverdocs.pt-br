---
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
title: Fsutil fsinfo
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 56e27c386451c561de8f62e523e2d1e59a8ce84c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844299"
---
# <a name="fsutil-fsinfo"></a>Fsutil fsinfo
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Lista todas as unidades, consulta o tipo de unidade, consulta informações de volume, consulta informações de volume específicas de NTFS ou consulta estatísticas do sistema de arquivos.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <VolumePath>
fsutil fsinfo [ntfsinfo] <RootPath>
fsutil fsinfo [statistics] <VolumePath>
fsutil fsinfo [volumeinfo] <RootPath>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|unidades|Lista todas as unidades no computador.|
|DriveType|Consulta uma unidade e lista seu tipo, por exemplo, unidade de CD-ROM.|
|NTFSInfo|Lista informações de volume específicas do NTFS para o volume especificado, como o número de setores, total de clusters, clusters livres e o início e término da zona MFT.|
|sectorinfo|Lista informações sobre o tamanho e o alinhamento do setor do hardware.|
|estatística|Lista as estatísticas do sistema de arquivos para o volume especificado, como metadados, arquivo de log e leituras e gravações de MFT.|
|volumeinfo|Lista informações para o volume especificado, como o sistema de arquivos, e se o volume dá suporte a nomes de arquivos que diferenciam maiúsculas de minúsculas, Unicode em nomes de arquivo, cotas de disco ou é um volume do DirectAccess (DAX).|
|< "VolumePath" >|Especifica a letra da unidade (seguida por dois-pontos).|
|< "RootPathname" >|Especifica a letra da unidade (seguida por dois-pontos) da unidade raiz.|

## <a name="examples"></a><a name="BKMK_examples"></a>Disso
Para listar todas as unidades no computador, digite:

```
fsutil fsinfo drives
```

Saída semelhante às seguintes exibições:

```
Drives: A:\ C:\ D:\ E:\       
```

Para consultar o tipo de unidade da unidade C, digite:

```
fsutil fsinfo drivetype c:
```

Os resultados possíveis da consulta incluem:

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

Para consultar as informações de volume do volume E, digite:

```
fsinfo volumeinfo e:\
```

Saída semelhante às seguintes exibições:

```
Volume Name :Volume
Serial Number : 0xd0b634d9
Max Component Length : 255
File System Name : NTFS
.
.
.
Supports Named Streams
Is DAX Volume
```

Para consultar a unidade F para obter informações de volume específicas do NTFS, digite:

```
fsutil fsinfo ntfsinfo f:
```

Saída semelhante às seguintes exibições:

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors :            0x00000000010ea04f
Total Clusters :            0x000000000021d409
.
.
.
Mft Zone End   :            0x0000000000004700       
```

Para consultar o hardware subjacente do sistema de arquivos para obter informações sobre o setor, digite:

```
fsinfo sectorinfo d:
```

Saída semelhante às seguintes exibições:

```
D:\>fsutil fsinfo sectorinfo d:
LogicalBytesPerSector :                                 4096
PhysicalBytesPerSectorForAtomicity :                    4096
.
.
.
Trim Not Supported
DAX capable
```

Para consultar as estatísticas do sistema de arquivos para a unidade E, digite:

```
fsinfo statistics e:
```

Saída semelhante às seguintes exibições:

```
File System Type :     NTFS
Version :              1
UserFileReads :        75021
UserFileReadBytes :    1305244512
.
.
.
LogFileWriteBytes :    180936704       
```

## <a name="additional-references"></a>Referências adicionais
- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[fsutil](Fsutil.md)


