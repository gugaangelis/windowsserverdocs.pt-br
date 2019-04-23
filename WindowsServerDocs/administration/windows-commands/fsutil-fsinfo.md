---
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
title: fsutil fsinfo
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 434dfde2286538367fb96d168b06983cb4357067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873037"
---
# <a name="fsutil-fsinfo"></a>fsutil fsinfo
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Lista todas as unidades, pesquisa o tipo de unidade, informações de volume de consultas, consulta informações de volume NTFS específicas ou estatísticas do sistema de arquivos de consulta.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <VolumePath>
fsutil fsinfo [ntfsinfo] <RootPath>
fsutil fsinfo [statistics] <VolumePath>
fsutil fsinfo [volumeinfo] <RootPath>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|Unidades|Lista todas as unidades no computador.|
|DriveType|Consulta de uma unidade e lista seu tipo, por exemplo unidade de CD-ROM.|
|ntfsinfo|Lista informações de volume específico do NTFS para o volume especificado, como o número de setores, total de clusters, clusters gratuitos e o início e final da zona de MFT.|
|sectorinfo|Lista informações sobre o tamanho de setor do hardware e o alinhamento.|
|Estatísticas|Lista as estatísticas de sistema para o volume especificado, como metadados, o arquivo de log e MFT leituras e gravações de arquivos.|
|volumeinfo|Lista informações para o volume especificado, como o sistema de arquivos e se o volume dá suporte a nomes de arquivo diferencia maiusculas de minúsculas, unicode em nomes de arquivos, cotas de disco, ou é um volume de DirectAccess (DAX).|
|<"VolumePath">|Especifica a letra da unidade (seguida por dois-pontos).|
|<"RootPathname">|Especifica a letra da unidade (seguida por dois-pontos) da unidade raiz.|

## <a name="BKMK_examples"></a>Exemplos
Para listar todas as unidades no computador, digite:

```
fsutil fsinfo drives
```

Saída semelhante à seguinte exibida:

```
Drives: A:\ C:\ D:\ E:\       
```

Para consultar o tipo de unidade da unidade C, digite:

```
fsutil fsinfo drivetype c:
```

Possíveis resultados da consulta incluem:

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

Para consultar as informações de volume para o volume E, digite:

```
fsinfo volumeinfo e:\
```

Saída semelhante à seguinte exibida:

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

Unidade de consulta F para obter informações de volume NTFS específicas, digite:

```
fsutil fsinfo ntfsinfo f:
```

Saída semelhante à seguinte exibida:

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors :            0x00000000010ea04f
Total Clusters :            0x000000000021d409
.
.
.
Mft Zone End   :            0x0000000000004700       
```

Para consultar a hardware subjacente do sistema de arquivos para obter informações do setor, digite:

```
fsinfo sectorinfo d:
```

Saída semelhante à seguinte exibida:

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

Para consultar as estatísticas de sistema de arquivos de unidade E, digite:

```
fsinfo statistics e:
```

Saída semelhante à seguinte exibida:

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

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)
[Fsutil](Fsutil.md)


