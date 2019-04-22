---
title: Simular a restauração
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 09b626939c13d4e38a983435b45d8c47ee2b93a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817247"
---
# <a name="simulate-restore"></a>Simular a restauração



Testa o envolvimento do gravador em sessões de restauração no computador sem emitir **PreRestore** ou **PostRestore** eventos para gravadores.

## <a name="syntax"></a>Sintaxe

```
simulate restore
```

## <a name="remarks"></a>Comentários

-   **Simular a restauração** é usado para testar se a restauração com gravadores pode ser bem-sucedida ou não.
-   Antes de usar **simular restauração**, você deve carregar um arquivo de metadados do DiskShadow usando o **carregar metadados** comando. Isso carrega os gravadores selecionados e os componentes para a restauração.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)