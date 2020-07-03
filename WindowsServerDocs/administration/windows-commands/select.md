---
title: select
description: Artigo de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6004d39e225b1ac4acd96b4108accff2ccfc485c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935931"
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

