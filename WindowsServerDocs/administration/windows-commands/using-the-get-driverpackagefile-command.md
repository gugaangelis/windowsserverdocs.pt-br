---
title: Usando o comando Get-DriverPackageFile
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21bbe17e56177da5cd2c1bf83c712d256cc794c8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363150"
---
# <a name="using-the-get-driverpackagefile-command"></a>Usando o comando Get-DriverPackageFile



Exibe informações sobre um pacote de driver, incluindo os drivers e arquivos que ele contém.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parâmetros

|         Parâmetro         |                              Descrição                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile: caminho de arquivo de \<Inf > | Especifica o caminho completo e o nome do arquivo do pacote de driver. inf. |
|    [/Architecture: {x86    |                                  Win64                                  |
|     [/Show: {drivers      |                                 Arquivos                                  |

## <a name="BKMK_examples"></a>Disso

Para exibir informações sobre um arquivo de driver, digite:
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)