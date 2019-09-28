---
title: bitsadmin nowrap
description: O tópico de comandos do Windows para **Bitsadmin nowrap** -trunca qualquer linha de texto de saída que se estenda além da borda mais à direita da janela de comando.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3806ec51161eeae498e3c9b367b2aacf0bd32c99
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381055"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Trunca qualquer linha de texto de saída que ultrapasse a borda mais à direita da janela de comando.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>Comentários

Por padrão, todas as opções, exceto a opção de **Monitor** , encapsulam a saída. Especifique a opção **nowrap** antes de outras opções.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera o estado do trabalho chamado *myDownloadJob* e não encapsula a saída
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)