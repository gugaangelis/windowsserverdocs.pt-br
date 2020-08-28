---
title: unexpose
description: Artigo de referência para unexpo, que não expõe uma cópia de sombra que foi exposta usando o comando Expose.
ms.topic: reference
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13e14941e2c67aa0361dcc0af2cdb1a36bf7e651
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036404"
---
# <a name="unexpose"></a>unexpose

Não expõe uma cópia de sombra que foi exposta usando o comando **Expo** . A cópia de sombra exposta pode ser especificada por sua ID de sombra, letra de unidade, compartilhamento ou ponto de montagem.



## <a name="syntax"></a>Sintaxe

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ShadowID>|Revela a cópia de sombra especificada pela ID de sombra fornecida.|
|\<Drive:>|Não expõe a cópia de sombra associada à letra da unidade especificada (por exemplo, a unidade P).|
|\<Share>|Revela a cópia de sombra associada ao compartilhamento especificado (por exemplo, \\ \\ *MachineName*) \) .|
|\<MountPoint>|Não expõe a cópia de sombra associada ao ponto de montagem especificado (por exemplo, C:\shadowcopy \) .|

## <a name="remarks"></a>Comentários

-   Você pode usar um alias existente ou uma variável de ambiente no lugar de *shadowid*. Use **Adicionar** sem parâmetros para ver os aliases existentes.

## <a name="examples"></a>Exemplos

Para revelar a cópia de sombra associada à unidade P, digite:
```
unexpose P:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)