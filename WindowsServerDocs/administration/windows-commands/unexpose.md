---
title: Cancele
description: Tópico de referência para unexpo, que não expõe uma cópia de sombra que foi exposta usando o comando Expose.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0caa412e5ff7de149f0a2bd8806f7141c368306
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721194"
---
# <a name="unexpose"></a>Cancele

Não expõe uma cópia de sombra que foi exposta usando o comando **Expo** . A cópia de sombra exposta pode ser especificada por sua ID de sombra, letra de unidade, compartilhamento ou ponto de montagem.



## <a name="syntax"></a>Sintaxe

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> shadowid|Revela a cópia de sombra especificada pela ID de sombra fornecida.|
|\<Unidade: >|Não expõe a cópia de sombra associada à letra da unidade especificada (por exemplo, a unidade P).|
|\<> de compartilhamento|Revela a cópia de sombra associada ao compartilhamento especificado (por exemplo, \\ \\ *MachineName*\)).|
|\<> de MountPoint|Não expõe a cópia de sombra associada ao ponto de montagem especificado (por exemplo,\)C:\shadowcopy.|

## <a name="remarks"></a>Comentários

-   Você pode usar um alias existente ou uma variável de ambiente no lugar de *shadowid*. Use **Adicionar** sem parâmetros para ver os aliases existentes.

## <a name="examples"></a>Exemplos

Para revelar a cópia de sombra associada à unidade P, digite:
```
unexpose P:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)