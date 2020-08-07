---
title: Get-DriverPackageFile
description: Artigo de referência para Get-DriverPackageFile, que exibe informações sobre um pacote de driver, incluindo os drivers e arquivos que ele contém.
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c80267f90608dca36ef9460eb23b66689022517
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879757"
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
| /InfFile:\<Inf File path> | Especifica o caminho completo e o nome do arquivo do pacote de driver. inf. |
|    [/Architecture: {x86    |                                  Win64                                  |
|     [/Show: {drivers      |                                 Arquivos                                  |

## <a name="examples"></a>Exemplos

Para exibir informações sobre um arquivo de driver, digite:
```
WDSUTIL /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)