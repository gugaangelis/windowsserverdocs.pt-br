---
title: reset
description: Artigo de referência para * * * *-
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f491a3d8c805ac6b3558f3778f6eba91a7551b82
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883654"
---
# <a name="reset"></a>reset



Redefine DiskShadow.exe para o estado padrão. A **redefinição** é especialmente útil para separar operações de DiskShadow compostos, como **criar**, **importar**, **fazer backup**ou **restaurar**.

## <a name="syntax"></a>Sintaxe

```
reset
```

## <a name="remarks"></a>Comentários

-   Quando você usa o comando **Reset** , perde o estado de comandos como **Add**, **set**, **Load**ou **Writer**. **Redefinir** também libera interfaces IVssBackupComponent e perde cópias de sombra não persistentes.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)