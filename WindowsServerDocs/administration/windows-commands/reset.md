---
title: Definido
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8903c300d12a019f8fb4aef6d367131a195d034
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371465"
---
# <a name="reset"></a>Definido



Redefine o DiskShadow. exe para o estado padrão. A **redefinição** é especialmente útil para separar operações de DiskShadow compostos, como **criar**, **importar**, **fazer backup**ou **restaurar**.

## <a name="syntax"></a>Sintaxe

```
reset
```

## <a name="remarks"></a>Comentários

-   Quando você usa o comando **Reset** , perde o estado de comandos como **Add**, **set**, **Load**ou **Writer**. **Redefinir** também libera interfaces IVssBackupComponent e perde cópias de sombra não persistentes.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)