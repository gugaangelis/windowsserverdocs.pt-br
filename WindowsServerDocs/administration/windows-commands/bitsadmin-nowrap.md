---
title: bitsadmin nowrap
description: Artigo de referência para o comando nowrap Bitsadmin, que trunca qualquer linha de texto de saída que se estenda além da borda mais à direita da janela de comando.
ms.topic: reference
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2e5724f3dedb808898cd070026e2e4a944d4731b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631424"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Trunca qualquer linha de texto de saída que ultrapasse a borda mais à direita da janela de comando. Por padrão, todas as opções, exceto a opção de **Monitor** , encapsulam a saída. Especifique a opção **nowrap** antes de outras opções.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /nowrap
```

## <a name="examples"></a>Exemplos

Para recuperar o estado do trabalho chamado *myDownloadJob* enquanto não estiver encapsulando a saída:

```
bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
