---
title: mask
description: Artigo de referência para o comando Mask, que remove cópias de sombra de hardware que foram importadas usando o comando Import.
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a893d32dca90169d51a04db66b3dc796cbc69a46
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886526"
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