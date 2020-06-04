---
title: mask
description: Tópico de referência para o comando Mask, que remove cópias de sombra de hardware que foram importadas usando o comando Import.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee01bb74b1fef1bb31a266c01a9e9bde7743691d
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354626"
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