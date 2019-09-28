---
title: selecionar
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7dc3bc8775f971968f096ba4344348e77c112cfa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384113"
---
# <a name="select"></a>selecionar



Desloca o foco para um disco, partição, volume ou VHD (disco rígido virtual).

## <a name="syntax"></a>Sintaxe

```
select disk
select partition
select volume
select vdisk
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Selecionar disco](select-disk.md)|Desloca o foco para um disco.|
|[Selecionar partição](select-partition.md)|Desloca o foco para uma partição.|
|[Selecionar volume](select-volume.md)|Desloca o foco para um volume.|
|[Selecionar vdisk](select-vdisk.md)|Desloca o foco para um VHD.|

## <a name="remarks"></a>Comentários

-   Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.
-   Se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

