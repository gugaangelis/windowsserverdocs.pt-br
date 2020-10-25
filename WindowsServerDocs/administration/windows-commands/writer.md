---
title: gravador
description: Artigo de referência para o comando do Writer, que verifica se um gravador ou componente está incluído ou exclui um gravador ou componente do procedimento de backup ou restauração.
ms.topic: reference
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d6af778ca49f8be88a08e04ecfe4165752cc796d
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524861"
---
# <a name="writer"></a>gravador

Verifica se um gravador ou componente está incluído ou exclui um gravador ou componente do procedimento de backup ou restauração. Se usado sem parâmetros, o **Writer** exibirá a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
writer verify [writer> | <component>]
writer exclude [<writer> | <component>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| verificar | Verifica se o gravador ou o componente especificado está incluído no procedimento de backup ou restauração. O procedimento de backup ou restauração falhará se o gravador ou o componente não estiver incluído. |
| excluir | Exclui o gravador ou componente especificado do procedimento de backup ou restauração. |

## <a name="examples"></a>Exemplos

Para verificar um gravador especificando seu GUID (para este exemplo, 4dc3bdd4-AB48-4d07-adb0-3bee2926fd7f), digite:

```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```

Para excluir um gravador com o nome *System Writer*, digite:

```
writer exclude System Writer
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
