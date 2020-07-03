---
title: unexpose
description: Artigo de referência para unexpo, que não expõe uma cópia de sombra que foi exposta usando o comando Expose.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 02edb1f2c9331a22473123f0327dbc84cb05a865
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937298"
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