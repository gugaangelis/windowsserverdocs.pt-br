---
title: selecionar
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9ad7051978f4b509f54bf783f71943b65617bc7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834649"
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

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Selecionar disco](select-disk.md)|Desloca o foco para um disco.|
|[Selecionar partição](select-partition.md)|Desloca o foco para uma partição.|
|[Selecionar volume](select-volume.md)|Desloca o foco para um volume.|
|[Selecionar vdisk](select-vdisk.md)|Desloca o foco para um VHD.|

## <a name="remarks"></a>Comentários

-   Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.
-   se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

