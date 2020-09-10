---
title: bitsadmin info
description: Artigo de referência do comando Bitsadmin info, que exibe informações de resumo sobre o trabalho especificado.
ms.topic: reference
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 815fdc719d584f7d25f88705056e4d5c0c3405aa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631537"
---
# <a name="bitsadmin-info"></a>bitsadmin info

Exibe informações de resumo sobre o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| /verbose | Opcional. Fornece informações detalhadas sobre cada trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar informações sobre o trabalho chamado *myDownloadJob*:

```
bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)
