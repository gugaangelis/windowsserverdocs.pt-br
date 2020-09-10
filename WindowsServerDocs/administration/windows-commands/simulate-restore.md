---
title: simulate restore
description: Artigo de referência para simular a restauração, que testa o envolvimento do gravador em sessões de restauração no computador sem emitir eventos prerestore ou createrestore para gravadores.
ms.topic: reference
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d72e4b473b3913bff744ff7a34b6508bde52ae0e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640925"
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