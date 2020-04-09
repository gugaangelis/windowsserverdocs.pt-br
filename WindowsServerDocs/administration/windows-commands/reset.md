---
title: redefinir
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c27ddd93d06670a30f797bd58dd396a9e7ce70a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835779"
---
# <a name="reset"></a>redefinir



Redefine o DiskShadow. exe para o estado padrão. A **redefinição** é especialmente útil para separar operações de DiskShadow compostos, como **criar**, **importar**, **fazer backup**ou **restaurar**.

## <a name="syntax"></a>Sintaxe

```
reset
```

## <a name="remarks"></a>Comentários

-   Quando você usa o comando **Reset** , perde o estado de comandos como **Add**, **set**, **Load**ou **Writer**. **Redefinir** também libera interfaces IVssBackupComponent e perde cópias de sombra não persistentes.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)