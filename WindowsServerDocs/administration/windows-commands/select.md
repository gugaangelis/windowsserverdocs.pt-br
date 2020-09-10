---
title: select
description: Artigo de referência para * * * *-
ms.topic: reference
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: da601731f9abe1f84f082fd91528db03e6a8b05a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638997"
---
# <a name="select"></a>select



Desloca o foco para um disco, partição, volume ou VHD (disco rígido virtual).

## <a name="syntax"></a>Sintaxe

```
select disk
select partition
select volume
select vdisk
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Selecionar disco](select-disk.md)|Desloca o foco para um disco.|
|[Selecionar partição](select-partition.md)|Desloca o foco para uma partição.|
|[Selecionar volume](select-volume.md)|Desloca o foco para um volume.|
|[Selecionar vdisk](select-vdisk.md)|Desloca o foco para um VHD.|

## <a name="remarks"></a>Comentários

-   Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.
-   Se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

