---
title: retain
description: Artigo de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 958ee0de7bd69c9391407ec6f4a832e1262746a2
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933077"
---
# <a name="retain"></a>retain



Prepara um volume simples dinâmico existente para ser usado como um volume do sistema ou de inicialização.

## <a name="syntax"></a>Sintaxe

```
retain
```

## <a name="remarks"></a>Comentários

-   Em um disco dinâmico MBR (registro mestre de inicialização), esse comando cria uma entrada de partição no registro mestre de inicialização.
-   Em um disco dinâmico GPT (tabela de partição GUID), esse comando cria uma entrada de partição na tabela de partição GUID.

## <a name="additional-references"></a>Referências adicionais

