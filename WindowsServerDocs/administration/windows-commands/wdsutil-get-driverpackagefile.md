---
title: WDSUTIL Get-driverpackagefile
description: Artigo de referência para WDSUTIL Get-driverpackagefile, que exibe informações sobre um pacote de driver, incluindo os drivers e arquivos que ele contém.
ms.topic: reference
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f85d57a81aa63f6ecb94b4b09a1614403aa19664
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729650"
---
# <a name="wdsutil-get-driverpackagefile"></a>WDSUTIL Get-driverpackagefile

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