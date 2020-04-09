---
title: mascara
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1cb92caacb955449c1baaad411fdbe4cdf05b73
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839639"
---
# <a name="mask"></a>mascara



Remove cópias de sombra de hardware que foram importadas usando o comando de **importação** .

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
mask <ShadowSetID>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|ShadowSetID|Remove cópias de sombra que pertencem à ID do conjunto de cópias de sombra especificado.|

## <a name="remarks"></a>Comentários

-   Você pode usar um alias existente ou uma variável de ambiente no lugar de *ShadowSetID*. Use **Adicionar** sem parâmetros para ver os aliases existentes.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para remover a cópia de sombra importada% Import_1%, digite:
```
mask %Import_1%
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)