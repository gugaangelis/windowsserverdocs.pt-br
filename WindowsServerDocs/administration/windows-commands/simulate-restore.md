---
title: simulate restore
description: Artigo de referência para o comando Simulate Restore, que testa se o envolvimento do gravador em sessões de restauração será bem-sucedido no computador sem emitir eventos prerestore ou createrestore para gravadores.
ms.topic: reference
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e9ca1760cd1d6a125267e152274ea26e1a9ae11f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718293"
---
# <a name="simulate-restore"></a>Simular restauração

Testa se o envolvimento do gravador em sessões de restauração será bem-sucedido no computador sem **emitir eventos de** **prerestore** ou de restauração nos gravadores.

> [!NOTE]
> Um arquivo de metadados do DiskShadow deve ser selecionado para que o comando **Simulate Restore** tenha sucesso. Use o [comando carregar metadados](load-metadata.md) para carregar os gravadores e componentes selecionados para a restauração.

## <a name="syntax"></a>Syntax

```
simulate restore
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando carregar metadados](load-metadata.md)
