---
title: simulate restore
description: Artigo de referência para simular a restauração, que testa o envolvimento do gravador em sessões de restauração no computador sem emitir eventos prerestore ou createrestore para gravadores.
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec2c49f0dfcb68f6b3b6ef0567778a4317e7dc24
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882339"
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