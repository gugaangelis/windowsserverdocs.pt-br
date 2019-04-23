---
title: rem
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7245aedb-ba6f-49bb-9f77-848c4853c68f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d78bbfe4434170078beb1fd3b3526b017a03e0be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857017"
---
# <a name="rem"></a>rem



Fornece uma maneira de adicionar comentários a um script.

## <a name="syntax"></a>Sintaxe

```
rem
```

## <a name="BKMK_examples"></a>Exemplos

Neste script de exemplo, **rem** é usado para fornecer um comentário sobre o que faz o script:
```
rem The commands in this script set up 3 drives.
rem The first drive is a primary partition and is
rem assigned the letter D. The second and third drives
rem are logical partitions, and are assigned letters
rem E and F.
create partition primary size=2048
assign d:
create partition extended
create partition logical size=2048
assign e:
create partition logical
assign f:
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

