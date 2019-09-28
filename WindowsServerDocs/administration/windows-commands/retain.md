---
title: manteve
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3b076e12c833645833f53a06476e62bbf44f2690
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384495"
---
# <a name="retain"></a>manteve



Prepara um volume simples dinâmico existente para ser usado como um volume do sistema ou de inicialização.

## <a name="syntax"></a>Sintaxe

```
retain
```

## <a name="remarks"></a>Comentários

-   Em um disco dinâmico MBR (registro mestre de inicialização), esse comando cria uma entrada de partição no registro mestre de inicialização.
-   Em um disco dinâmico GPT (tabela de partição GUID), esse comando cria uma entrada de partição na tabela de partição GUID.

#### <a name="additional-references"></a>Referências adicionais

