---
title: reset
description: Artigo de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a412eff7bdf432608a999edb4531074ed5e8f26
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933095"
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