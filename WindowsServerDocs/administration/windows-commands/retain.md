---
title: Reter
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b437e9f0c8d671e4378311d450aa0ac7639219f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852167"
---
# <a name="retain"></a>Reter



Prepara um volume dinâmico simple existente a ser usado como uma inicialização ou o volume do sistema.

## <a name="syntax"></a>Sintaxe

```
retain
```

## <a name="remarks"></a>Comentários

-   Em um disco dinâmico do MBR (registro) de mestre de inicialização, esse comando cria uma entrada de partição no registro mestre de inicialização.
-   Em um disco partição GUID (GPT) da tabela dinâmico, este comando cria uma entrada de partição na tabela de partição GUID.

#### <a name="additional-references"></a>Referências adicionais

