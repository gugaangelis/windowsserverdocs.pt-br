---
title: unexpose
description: Artigo de referência para unexpo, que não expõe uma cópia de sombra que foi exposta usando o comando Expose.
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c315639746db84d49afd72fc2be89e757c8fb95
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87897069"
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