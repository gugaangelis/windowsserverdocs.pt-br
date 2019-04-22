---
title: disco de detalhe
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2c7a5063edf3cb2e190e8aec957e1b571c1f15bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819097"
---
# <a name="detail-disk"></a>disco de detalhe



Exibe as propriedades do disco selecionado e os volumes existentes nele.

## <a name="syntax"></a>Sintaxe

```
detail disk
```

## <a name="remarks"></a>Comentários

-   Um disco deve ser selecionado para essa operação seja bem-sucedida. Use o **Selecionar disco** comando para selecionar um disco e mudar o foco a ele.
-   Se o disco selecionado for um disco rígido virtual (VHD), **disco detalhes** relata o tipo de barramento do disco como virtuais.

## <a name="BKMK_examples"></a>Exemplos

Para ver as propriedades do disco selecionado e informações sobre os volumes no disco, digite:
```
detail disk
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

