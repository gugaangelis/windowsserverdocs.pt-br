---
title: selecionar
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4c3723dd414adca68c22011ef3f6be02eb6531d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889897"
---
# <a name="select"></a>selecionar



Desloca o foco para um disco, partição, volume ou disco rígido virtual (VHD).

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
|[Selecione o disco](select-disk.md)|Desloca o foco para um disco.|
|[Selecione a partição](select-partition.md)|Desloca o foco para uma partição.|
|[Selecione o volume](select-volume.md)|Desloca o foco para um volume.|
|[Selecionar o vdisk](select-vdisk.md)|Desloca o foco para um VHD.|

## <a name="remarks"></a>Comentários

-   Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.
-   Se uma partição é selecionada com um volume correspondente, o volume será selecionado automaticamente.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

