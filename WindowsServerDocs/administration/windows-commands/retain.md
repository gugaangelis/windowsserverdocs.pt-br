---
title: manteve
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7391c0b60d07cc2eb5a6230b283ac180cc028508
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722337"
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

## <a name="additional-references"></a>Referências adicionais

