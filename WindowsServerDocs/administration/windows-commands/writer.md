---
title: Gravador
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8aee4ecca85c7d5f46ee79f3ad928b746c02e7bb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439980"
---
# <a name="writer"></a>Gravador



Verifica se um gravador ou o componente está incluído ou exclui um componente ou o gravador do procedimento de backup ou restauração. Se usado sem parâmetros, **gravador** exibe a Ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>Parâmetros

| Parâmetro  |                                                                                      Descrição                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | Verifica se o gravador especificado ou o componente está incluído no procedimento de backup ou restauração. O procedimento de backup ou restauração falhará se o gravador ou o componente não está incluído. |
|  exclude   |                                                   Exclui o gravador especificado ou o componente do procedimento de backup ou restauração.                                                    |
| [\<Writer> |                                                                                     <Component>]                                                                                      |

## <a name="BKMK_examples"></a>Exemplos

Para verificar se um gravador, especificando seu GUID (por exemplo, 4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f), digite:
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Para excluir um gravador com o nome "Gravador do sistema", digite:
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)