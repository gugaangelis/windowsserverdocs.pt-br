---
title: simulate restore
description: Artigo de referência para simular a restauração, que testa o envolvimento do gravador em sessões de restauração no computador sem emitir eventos prerestore ou createrestore para gravadores.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cc247848ef4fac1e3a6537247f640f3533c2bcd
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936352"
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