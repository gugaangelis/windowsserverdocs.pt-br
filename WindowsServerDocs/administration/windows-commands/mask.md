---
title: mask
description: Artigo de referência para o comando Mask, que remove cópias de sombra de hardware que foram importadas usando o comando Import.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2839fce0a64f187c1445a5f6a4af6c5f0ebc9fb8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922104"
---
# <a name="mask"></a>mask

Remove cópias de sombra de hardware que foram importadas usando o comando de **importação** .

## <a name="syntax"></a>Sintaxe

```
mask <shadowsetID>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| shadowsetID | Remove cópias de sombra que pertencem à ID do conjunto de cópias de sombra especificado. |

#### <a name="remarks"></a>Comentários

- Você pode usar um alias existente ou uma variável de ambiente no lugar de *ShadowSetID*. Use **Adicionar** sem parâmetros para ver os aliases existentes.

### <a name="examples"></a>Exemplos

Para remover a cópia de sombra importada *% Import_1%*, digite:

```
mask %Import_1%
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)