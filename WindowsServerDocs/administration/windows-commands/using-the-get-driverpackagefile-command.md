---
title: Get-DriverPackageFile
description: O tópico de comandos do Windows para Get-DriverPackageFile, que exibe informações sobre um pacote de driver, incluindo os drivers e arquivos que ele contém.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d485a24479aa857270968a1bff7bd55a014347a3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831029"
---
# <a name="get-driverpackagefile"></a>Get-DriverPackageFile

Exibe informações sobre um pacote de driver, incluindo os drivers e arquivos que ele contém.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>Parâmetros

|         Parâmetro         |                              Descrição                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile:\<caminho do arquivo inf > | Especifica o caminho completo e o nome do arquivo do pacote de driver. inf. |
|    [/Architecture: {x86    |                                  Win64                                  |
|     [/Show: {drivers      |                                 Files                                  |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para exibir informações sobre um arquivo de driver, digite:
```
WDSUTIL /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)