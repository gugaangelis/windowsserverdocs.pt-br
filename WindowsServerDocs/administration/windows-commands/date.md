---
title: date
description: Artigo de referência do comando date, que exibe ou define a data do sistema. Se usado sem parâmetros,
ms.topic: reference
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ca9e8b9305132e9af03308d2e0ca0cc20ed89469
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628970"
---
# <a name="date"></a>date

Exibe ou define a data do sistema. Se usado sem parâmetros, **Data** exibe a configuração de data atual do sistema e solicita que você insira uma nova data.

>[!IMPORTANT]
> Você deve ser um administrador para usar este comando.

## <a name="syntax"></a>Sintaxe

```
date [/t | <month-day-year>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<month-day-year>` | Define a data especificada, em que *month* é o mês (um ou dois dígitos, incluindo os valores de 1 a 12), *Day* é o dia (um ou dois dígitos, incluindo os valores de 1 a 31) e *year* é o ano (dois ou quatro dígitos, incluindo os valores de 00 a 99 ou 1980 a 2099). Você deve separar valores para *mês*, *dia*e *ano* com pontos (.), hifens (-) ou barras (/).<p>**Observação:** Lembre-se de que, se você usar dois dígitos para representar o ano, os valores 80-99 corresponderão a 1980 até 1999. |
| /t | Exibe a data atual sem solicitar uma nova data. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Se as extensões de comando estiverem habilitadas, para exibir a data atual do sistema, digite:

```
date /t
```

Para alterar a data atual do sistema para 3 de agosto de 2007, você pode digitar qualquer um dos seguintes:

```
date 08.03.2007
date 08-03-07
date 8/3/07
```

Para exibir a data atual do sistema, seguido de um prompt para inserir uma nova data, digite:

```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yyyy)
```

Para manter a data atual e retornar ao prompt de comando, pressione **Enter**. Para alterar a data atual, digite a nova data e pressione **Enter**.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)