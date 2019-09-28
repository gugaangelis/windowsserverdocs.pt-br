---
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
title: Fsutil volume
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c4496cfec94823ae177bc6de4fac83dc977fb61d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376702"
---
# <a name="fsutil-volume"></a>Fsutil volume
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Desmonta um volume ou consulta a unidade de disco rígido para determinar a quantidade de espaço livre disponível no momento na unidade de disco rígido ou qual arquivo está usando um cluster específico.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fsutil volume [allocationreport] <VolumePath>
fsutil volume [diskfree] <VolumePath>
fsutil volume [dismount] <VolumePath>
fsutil volume [filelayout] <VolumePath> <fileid>
fsutil volume [list]
fsutil volume [querycluster] <VolumePath> <Cluster> [<Cluster>] … …
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|allocationreport|Exibe informações sobre como o armazenamento é usado em um determinado volume.|
|\<VolumePath >|Especifica a letra da unidade (seguida por dois-pontos).|
|diskfree|Consulta a unidade de disco rígido para determinar a quantidade de espaço livre nela.|
|desmontagem|Desmonta um volume.|
|filelayout|Exibe os metadados NTFS para o arquivo fornecido.|
|\<fileid >|Especifica a ID do arquivo.|
|lista|Lista todos os volumes no sistema.|
|querycluster|Localiza qual arquivo está usando um cluster especificado. Você pode especificar vários clusters com o parâmetro **querycluster** .<br /><br />Esse parâmetro se aplica a:  Windows Server 2008 R2 e Windows 7.|
|\<cluster >|Especifica o número do cluster lógico (LCN).|

## <a name="BKMK_examples"></a>Disso
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

#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Como funciona o NTFS](https://go.microsoft.com/fwlink/?LinkId=183396)


