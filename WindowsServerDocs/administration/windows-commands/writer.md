---
title: gravador
description: Artigo de referência para o gravador, que verifica se um gravador ou componente está incluído ou exclui um gravador ou componente do procedimento de backup ou restauração.
ms.topic: reference
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 061def6ad0aa0240f1c50b92fa567bc1636650d8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628498"
---
# <a name="writer"></a>gravador



Verifica se um gravador ou componente está incluído ou exclui um gravador ou componente do procedimento de backup ou restauração. Se usado sem parâmetros, o **Writer** exibirá a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro  |                                                                                      Descrição                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verificar   | Verifica se o gravador ou o componente especificado está incluído no procedimento de backup ou restauração. O procedimento de backup ou restauração falhará se o gravador ou o componente não estiver incluído. |
|  excluir   |                                                   Exclui o gravador ou componente especificado do procedimento de backup ou restauração.                                                    |
| [\<Writer> |                                                                                     <Component>]                                                                                      |

## <a name="examples"></a>Exemplos

Para verificar um gravador especificando seu GUID (para este exemplo, 4dc3bdd4-AB48-4d07-adb0-3bee2926fd7f), digite:
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Para excluir um gravador com o nome System Writer, digite:
```
writer exclude System Writer
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)