---
title: volume de detalhes
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8cd5889bfd2aea835cb64ef1a4076faee0f39b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378471"
---
# <a name="detail-volume"></a>volume de detalhes



Exibe os discos nos quais o volume atual reside.

## <a name="syntax"></a>Sintaxe

```
detail volume
```

## <a name="remarks"></a>Comentários

-   Um volume deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.
-   Os detalhes do volume não são aplicáveis a volumes somente leitura, como uma unidade de DVD-ROM ou CD-ROM.

## <a name="BKMK_examples"></a>Disso

Para ver todos os discos nos quais reside o volume atual, digite:
```
detail volume
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

