---
title: retain
description: Artigo de referência para * * * *-
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a0205f3b67bd99ca590c7ffc6fbd04b0eefd94f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883639"
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

