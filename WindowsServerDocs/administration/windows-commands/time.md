---
title: time
description: Artigo de referência para o comando time, que exibe ou define a hora do sistema.
ms.topic: reference
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b69db2fce1af52d8f284e79fe83f5ac20c398cd6
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156478"
---
# <a name="time"></a>time

Exibe ou define a hora do sistema. Se usado sem parâmetros, **time** exibe a hora atual do sistema e solicita que você insira uma nova hora.

> [!NOTE]
> Você deve ser um administrador para alterar a hora atual.

## <a name="syntax"></a>Sintaxe

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<HH>[:<MM>[:<SS>[.<NN>]]] [am | pm]` | Define a hora do sistema para a nova hora especificada, em que *hh* está em horas (obrigatório), *mm* é em minutos e *SS* é em segundos. *NN* pode ser usado para especificar centésimos de segundo. Você deve separar valores para *hh*, *mm*e *SS* com dois-pontos (:). *SS* e *NN* devem ser separados por um ponto (.).<p>Se **am** ou **PM** não for especificado, a **hora** usará o formato de 24 horas por padrão. |
| /t | Exibe a hora atual sem solicitar um novo horário. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os valores de *hh* válidos são de 0 a 24.

- Os valores *mm* e *SS* válidos são de 0 a 59.

## <a name="examples"></a>Exemplos

Se as extensões de comando estiverem habilitadas, para exibir a hora atual do sistema, digite:

```
time /t
```

Para alterar a hora atual do sistema para 5:30 PM, digite um dos seguintes:

```
time 17:30:00
time 5:30 pm
```

Para exibir a hora atual do sistema, seguida de um prompt para inserir uma nova hora, digite:

```
The current time is: 17:33:31.35
Enter the new time:
```

Para manter a hora atual e retornar ao prompt de comando, pressione ENTER. Para alterar a hora atual, digite a nova hora e pressione ENTER.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
