---
title: selecionar comandos
description: Artigo de referência para os comandos SELECT, que desloca o foco para um disco, partição, volume ou VHD (disco rígido virtual).
ms.topic: reference
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2b322fc7bf9355e64fbe14a0823c85dddadd6171
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389174"
---
# <a name="select-commands"></a>selecionar comandos

Desloca o foco para um disco, partição, volume ou VHD (disco rígido virtual).

## <a name="syntax"></a>Sintaxe

```
select disk
select partition
select vdisk
select volume
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| [Selecionar disco](select-disk.md) | Desloca o foco para um disco. |
| [Selecionar partição](select-partition.md) | Desloca o foco para uma partição. |
| [Selecionar vdisk](select-vdisk.md) | Desloca o foco para um VHD. |
| [Selecionar volume](select-volume.md) | Desloca o foco para um volume. |

#### <a name="remarks"></a>Comentários

- Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.

- Se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
