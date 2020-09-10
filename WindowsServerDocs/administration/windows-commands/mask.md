---
title: mask
description: Artigo de referência para o comando Mask, que remove cópias de sombra de hardware que foram importadas usando o comando Import.
ms.topic: reference
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ee0e4207a7c5cf6ad81ece39e9134881ad3c0239
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633714"
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