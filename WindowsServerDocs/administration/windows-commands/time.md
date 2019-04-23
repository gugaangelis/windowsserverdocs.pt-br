---
title: time
description: Saiba como definir e exibir a hora do sistema.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5c1f43be98a19c4b150c247cc7fd48d62edeb5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861907"
---
# <a name="time"></a>time



Exibe ou define a hora do sistema. Se usado sem parâmetros, **tempo** exibe a hora atual do sistema e solicita que você insira um novo tempo.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<HH>[:\<MM>[:\<SS>[.\<NN>]]] [am\|pm]|Define a hora do sistema para a nova hora especificada, onde *HH* é em horas (obrigatório), *MM* é em minutos, e *SS* é em segundos. *NN* pode ser usado para especificar os centésimos de segundo. Se **estou** ou **pm** não for especificado, **tempo** usa o formato de 24 horas por padrão.|
|/t|Exibe a hora atual sem notificá-lo para um novo tempo.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para alterar a hora atual, você deve ter credenciais administrativas.
-   Você deve separar os valores para *HH*, *MM*, e *SS* com dois-pontos (:). *SS* e *NN* devem ser separados por um ponto (.).
-   Válido *HH* os valores são 0 a 24.
-   Válido *MM* e *SS* os valores são 0 a 59.

## <a name="BKMK_examples"></a>Exemplos

Se as extensões de comando estiverem habilitadas, para exibir a hora atual do sistema, digite:
```
time /t
```
Para alterar a hora atual do sistema às 17h: 30, digite o seguinte:
```
time 17:30:00
time 5:30 pm
```
Para exibir a hora atual do sistema, seguida de um prompt para inserir um novo período, digite:
```
The current time is: 17:33:31.35
Enter the new time:
```
Para manter a hora atual e retornar ao prompt de comando, pressione ENTER. Para alterar a hora atual, digite a nova hora e, em seguida, pressione ENTER.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)