---
title: Simular restauração
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6652fea4e74c706fcc03b8a547fab771a7c0191
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370878"
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

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)