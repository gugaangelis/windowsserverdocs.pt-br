---
title: Usando o comando get-DriverPackageFile
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ed9518fae07745502d01dc0084b7443a1332db83
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859797"
---
# <a name="using-the-get-driverpackagefile-command"></a>Usando o comando get-DriverPackageFile



Exibe informações sobre um pacote de driver, incluindo os drivers e arquivos que ele contém.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/ InfFile:\<caminho do arquivo Inf >|Especifica o nome de arquivo e caminho completo do arquivo. inf de pacote do driver.|
|[/Architecture:{x86 | ia64 | x64}]|Especifica a arquitetura do pacote de driver.|
|[/Show: {Drivers | Arquivos | All}]|Indica as informações do pacote para exibir. Se **/Mostrar** não for especificado, o padrão é retornar somente o driver de metadados do pacote. **Drivers** exibe a lista de drivers no pacote. **Arquivos** exibe a lista de arquivos no pacote. **Todos os** exibe drivers e arquivos.|

## <a name="BKMK_examples"></a>Exemplos

Para exibir informações sobre um arquivo de driver, digite:
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)