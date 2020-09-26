---
title: serverweroptin
description: Artigo de referência para o comando serverWerOptin, que permite que você ative o relatório de erros.
ms.topic: reference
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c8a2fe32634704a22f12430b8516ed7f6cae4e82
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389126"
---
# <a name="serverweroptin"></a>serverweroptin

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que você ative o relatório de erros.

## <a name="syntax"></a>Sintaxe

```
serverweroptin [/query] [/detailed] [/summary]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /Query | Verifica sua configuração atual. |
| / detalhadas | Especifica o envio automático de relatórios detalhados. |
| Resumo | Especifica o envio automático de relatórios de resumo. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para verificar a configuração atual, digite:

```
serverweroptin /query
```

Para enviar automaticamente relatórios detalhados, digite:

```
serverweroptin /detailed
```

Para enviar relatórios de resumo automaticamente, digite:

```
serverweroptin /summary
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
