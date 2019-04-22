---
title: bitsadmin nowrap
description: Tópico de comandos do Windows para **bitsadmin nowrap** -trunca qualquer linha de texto de saída estender além da borda mais à direita da janela de comando.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4130f606a6b1874e1ea31952160de44d6e09c6b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822917"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Trunca qualquer linha de texto de saída estender além da borda mais à direita da janela de comando.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>Comentários

Por padrão, todos os comutadores, exceto o **Monitor** alternar, encapsule a saída. Especifique o **NoWrap** alternar antes de outras opções.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera o estado do trabalho nomeado *myDownloadJob* e não se ajustam a saída
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)