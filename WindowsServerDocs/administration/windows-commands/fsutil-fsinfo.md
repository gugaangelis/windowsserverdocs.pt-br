---
title: Fsutil fsinfo
description: Tópico de referência para o comando fsutil fsinfo, que lista todas as unidades, consulta o tipo de unidade, consulta informações de volume, consulta informações de volume específicas de NTFS ou consulta estatísticas do sistema de arquivos.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 04d64bf0d7d29290cfc5e1ca88a013432322dbc1
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435816"
---
# <a name="fsutil-fsinfo"></a>fsutil fsinfo

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Lista todas as unidades, consulta o tipo de unidade, consulta informações de volume, consulta informações de volume específicas de NTFS ou consulta estatísticas do sistema de arquivos.

## <a name="syntax"></a>Sintaxe

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <volumepath>
fsutil fsinfo [ntfsinfo] <rootpath>
fsutil fsinfo [statistics] <volumepath>
fsutil fsinfo [volumeinfo] <rootpath>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |------------ |
| unidades | Lista todas as unidades no computador. |
| DriveType | Consulta uma unidade e lista seu tipo, por exemplo, unidade de CD-ROM. |
| NTFSInfo | Lista informações de volume específicas do NTFS para o volume especificado, como o número de setores, total de clusters, clusters livres e o início e término da zona MFT. |
| sectorinfo | Lista informações sobre o tamanho e o alinhamento do setor do hardware. |
| estatísticas | Lista as estatísticas do sistema de arquivos para o volume especificado, como metadados, arquivo de log e leituras e gravações de MFT. |
| volumeinfo | Lista informações para o volume especificado, como o sistema de arquivos, e se o volume dá suporte a nomes de arquivos que diferenciam maiúsculas de minúsculas, Unicode em nomes de arquivo, cotas de disco ou é um volume do DirectAccess (DAX). |
| `<volumepath>:` | Especifica a letra da unidade (seguida por dois-pontos). |
| `<rootpath>:` | Especifica a letra da unidade (seguida por dois-pontos) da unidade raiz. |

### <a name="examples"></a>Exemplos

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
Volume Name : Volume
Serial Number : 0xd0b634d9
Max Component Length : 255
File System Name : NTFS
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
Number Sectors : 0x00000000010ea04f
Total Clusters : 0x000000000021d409
Mft Zone End : 0x0000000000004700
```

Para consultar o hardware subjacente do sistema de arquivos para obter informações sobre o setor, digite:

```
fsinfo sectorinfo d:
```

Saída semelhante às seguintes exibições:

```
D:\>fsutil fsinfo sectorinfo d:
LogicalBytesPerSector : 4096
PhysicalBytesPerSectorForAtomicity : 4096
Trim Not Supported
DAX capable
```

Para consultar as estatísticas do sistema de arquivos para a unidade E, digite:

```
fsinfo statistics e:
```

Saída semelhante às seguintes exibições:

```
File System Type : NTFS
Version : 1
UserFileReads : 75021
UserFileReadBytes : 1305244512
LogFileWriteBytes : 180936704
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
