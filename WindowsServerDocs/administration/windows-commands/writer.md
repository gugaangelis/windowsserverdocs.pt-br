---
title: escritor
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6c00f6067cd5f6cf741cddbd6d62c5bcbb1f37a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361863"
---
# <a name="writer"></a>escritor



verifica se um gravador ou componente está incluído ou exclui um gravador ou componente do procedimento de backup ou restauração. Se usado sem parâmetros, o **Writer** exibirá a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>Parâmetros

| Parâmetro  |                                                                                      Descrição                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | Verifica se o gravador ou o componente especificado está incluído no procedimento de backup ou restauração. O procedimento de backup ou restauração falhará se o gravador ou o componente não estiver incluído. |
|  exclude   |                                                   Exclui o gravador ou componente especificado do procedimento de backup ou restauração.                                                    |
| [\<Writer > |                                                                                     <Component>]                                                                                      |

## <a name="BKMK_examples"></a>Disso

Para verificar um gravador especificando seu GUID (para este exemplo, 4dc3bdd4-AB48-4d07-adb0-3bee2926fd7f), digite:
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Para excluir um gravador com o nome "gravador do sistema", digite:
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)