---
title: Redefinir
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0bd9b6735697cbcefdcf68dc3d4a53a6870a7a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850957"
---
# <a name="reset"></a>Redefinir



Redefine DiskShadow.exe para o estado padrão. **Redefinir** é especialmente útil na separação de operações do DiskShadow compostas, como **crie**, **importar**, **backup**, ou **restaurar**.

## <a name="syntax"></a>Sintaxe

```
reset
```

## <a name="remarks"></a>Comentários

-   Quando você usa o **redefinição** de comando, você perde estado de comandos, como **adicione**, **definir**, **carregar**, ou **gravador**. **Redefinir** também libera IVssBackupComponent interfaces e perder as cópias de sombra não persistente.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)