---
title: serverceipoptin
description: Artigo de referência para o serverceipoptin, que permite que você participe do Programa de Aperfeiçoamento da Experiência do Usuário (CEIP).
ms.topic: reference
ms.assetid: 3d7d7fa7-0689-4797-b802-36fe260d21a0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6df387158a9630ab767dda655346836355fae936
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389164"
---
# <a name="serverceipoptin"></a>serverceipoptin

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que você participe do Programa de Aperfeiçoamento da Experiência do Usuário (CEIP).

## <a name="syntax"></a>Sintaxe

```
serverceipoptin [/query] [/enable] [/disable]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /Query | Verifica sua configuração atual. |
| /Enable | Ativa sua participação no CEIP. |
| /Disable | Desativa sua participação no CEIP. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para verificar as configurações atuais, digite:

```
serverceipoptin /query
```

Para ativar sua participação, digite:

```
serverceipoptin /enable
```

Para desativar sua participação, digite:

```
serverceipoptin /disable
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
