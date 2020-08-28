---
title: fsutil volume
description: Artigo de referência para o comando de volume fsutil, que desmonta um volume ou consulta a unidade de disco rígido para determinar a quantidade de espaço livre disponível no momento na unidade de disco rígido ou qual arquivo está usando um cluster específico.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: c51d7c993199c395d2074fc1db393239a9b4b603
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032884"
---
# <a name="fsutil-volume"></a>fsutil volume

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Desmonta um volume ou consulta a unidade de disco rígido para determinar a quantidade de espaço livre disponível no momento na unidade de disco rígido ou qual arquivo está usando um cluster específico.

## <a name="syntax"></a>Sintaxe

```
fsutil volume [allocationreport] <volumepath>
fsutil volume [diskfree] <volumepath>
fsutil volume [dismount] <volumepath>
fsutil volume [filelayout] <volumepath> <fileID>
fsutil volume [list]
fsutil volume [querycluster] <volumepath> <cluster> [<cluster>] … …
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| allocationreport | Exibe informações sobre como o armazenamento é usado em um determinado volume. |
| `<volumepath>` | Especifica a letra da unidade (seguida por dois-pontos). |
| diskfree | Consulta a unidade de disco rígido para determinar a quantidade de espaço livre nela. |
| desmontagem | Desmonta um volume. |
| filelayout | Exibe os metadados NTFS para o arquivo fornecido. |
| `<fileID>` | Especifica a ID do arquivo. |
| list | Lista todos os volumes no sistema. |
| querycluster | Localiza qual arquivo está usando um cluster especificado. Você pode especificar vários clusters com o parâmetro **querycluster** . |
| `<cluster>` | Especifica o número do cluster lógico (LCN). |

### <a name="examples"></a>Exemplos

Para exibir um relatório de clusters alocados, digite:

```
fsutil volume allocationreport C:
```

Para desmontar um volume na unidade C, digite:

```
fsutil volume dismount c:
```

Para consultar a quantidade de espaço livre de um volume na unidade C, digite:

```
fsutil volume diskfree c:
```

Para exibir todas as informações sobre um ou mais arquivos especificados, digite:

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

Para listar os volumes em disco, digite:

```
fsutil volume list
```

Para localizar os arquivos que estão usando os clusters, especificados pelos números de cluster lógico 50 e 0x2000, na unidade C, digite:

```
fsutil volume querycluster C: 50 0x2000
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [Como funciona o NTFS](/previous-versions/windows/it-pro/windows-server-2003/cc781134(v=ws.10))
