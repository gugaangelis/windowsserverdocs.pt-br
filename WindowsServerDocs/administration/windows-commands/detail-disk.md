---
title: disco de detalhes
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff78a3f9e27cde35a7e19bdf1565c515a127261b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378581"
---
# <a name="detail-disk"></a>disco de detalhes



Exibe as propriedades do disco selecionado e os volumes existentes nele.

## <a name="syntax"></a>Sintaxe

```
detail disk
```

## <a name="remarks"></a>Comentários

-   Um disco deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.
-   Se o disco selecionado for um VHD (disco rígido virtual), o **disco de detalhes** relatará o tipo de barramento do disco como virtual.

## <a name="BKMK_examples"></a>Disso

Para ver as propriedades do disco selecionado e informações sobre os volumes no disco, digite:
```
detail disk
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

