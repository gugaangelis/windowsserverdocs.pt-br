---
title: Cancele a exposição
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe9cb5dfd8ae6c71fdc72ddc1e8421229f98f5d0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837467"
---
# <a name="unexpose"></a>Cancele a exposição



Unexposes uma cópia de sombra foi exposta por meio de **expor** comando. A cópia de sombra exposto pode ser especificada por sua ID de sombra, letra da unidade, compartilhamento ou ponto de montagem.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ShadowID>|Unexposes a cópia de sombra especificada pela ID de sombra fornecido.|
|\<Drive:>|Unexposes a cópia de sombra associada com a letra da unidade especificada (por exemplo, a unidade P).|
|\<Compartilhamento >|Unexposes a cópia de sombra associada com o compartilhamento especificado (por exemplo, \\ \\ *MachineName*\).|
|\<MountPoint>|Unexposes a cópia de sombra associada com o ponto de montagem especificado (por exemplo, C:\shadowcopy\).|

## <a name="remarks"></a>Comentários

-   Você pode usar um alias existente ou uma variável de ambiente no lugar de *ShadowID*. Use **adicionar** sem parâmetros para ver os aliases existentes.

## <a name="BKMK_examples"></a>Exemplos

Para não expor a cópia de sombra associada à unidade P, digite:
```
unexpose P:
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)