---
title: tzutil
description: Artigo de referência para o comando tzutil, que exibe o utilitário de fuso horário do Windows.
ms.topic: reference
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8778758c2e3b72827a7dba5844539d27519a19da
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156332"
---
# <a name="tzutil"></a>tzutil

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe o utilitário de fuso horário do Windows.

## <a name="syntax"></a>Sintaxe

```
tzutil [/?] [/g] [/s <timezoneID>[_dstoff]] [/l]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /g | Exibe a ID do fuso horário atual. |
| /s `<timezoneID>[_dstoff]` | Define o fuso horário atual usando a ID de fuso horário especificada. O sufixo **_dstoff** desabilita os ajustes de horário de verão para o fuso horário (quando aplicável). Seu valor deve estar entre aspas. |
| /l | Lista todas as IDs de fuso horário e nomes de exibição válidos. A saída aparece como:<ul><li>`<display name>`</li><li>`<time zone ID>`</li></ul> |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

Um código de saída **0** indica que o comando foi concluído com êxito.

## <a name="examples"></a>Exemplos

Para exibir a ID do fuso horário atual, digite:

```
tzutil /g
```

Para definir o fuso horário atual como hora padrão do Pacífico, digite:

```
tzutil /s "Pacific Standard time"
```

Para definir o fuso horário atual como hora padrão do Pacífico e desabilitar os ajustes de horário de verão, digite:

```
tzutil /s "Pacific Standard time_dstoff"
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
