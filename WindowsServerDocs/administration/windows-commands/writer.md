---
title: escritor
description: O tópico de comandos do Windows para o gravador, que verifica se um gravador ou componente está incluído ou exclui um gravador ou componente do procedimento de backup ou restauração.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb13de162b8e5eb8150d145a4afacccf47bb25f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828979"
---
# <a name="writer"></a>escritor



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
| [\<Writer > |                                                                                     <Component>]                                                                                      |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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