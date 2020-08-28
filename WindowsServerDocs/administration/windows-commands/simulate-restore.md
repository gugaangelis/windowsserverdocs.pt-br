---
title: simulate restore
description: Artigo de referência para simular a restauração, que testa o envolvimento do gravador em sessões de restauração no computador sem emitir eventos prerestore ou createrestore para gravadores.
ms.topic: reference
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5640f69f421d65588251ff1e15b63cbeaffde613
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036974"
---
# <a name="simulate-restore"></a>Simular restauração

Testa o envolvimento do gravador em sessões de restauração no computador sem emitir eventos **Prerestore** ou **createrestore** para gravadores.

## <a name="syntax"></a>Sintaxe

```
simulate restore
```

## <a name="remarks"></a>Comentários

-   O **Simulate Restore** é usado para testar se a restauração com gravadores pode ou não ser bem-sucedida.
-   Antes de usar a **simulação de restauração**, você deve carregar um arquivo de metadados do DiskShadow usando o comando **carregar metadados** . Isso carrega os gravadores e componentes selecionados para a restauração.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)