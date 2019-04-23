---
title: date
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e774f7bfabb9b574255691dd97d2cfff36f034e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877947"
---
# <a name="date"></a>date



Exibe ou define a data do sistema. Se usado sem parâmetros, **data** exibe a configuração de data atual do sistema e solicita que você insira uma nova data.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
date [/t | <Month-Day-Year>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Mês-dia-ano >|Define a data especificada, onde *mês* é o mês (um ou dois dígitos), *dia* é o dia (um ou dois dígitos), e *ano* é o ano (duas ou quatro dígitos).|
|/t|Exibe a data atual, sem notificá-lo para uma nova data.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para alterar a data atual, você deve ter credenciais administrativas.
-   Você deve separar os valores para *mês*, *dia*, e *ano* com pontos (.), hifens (-) ou barra (/) de marca.
-   Válido *mês* valores variam de 1 a 12.
-   Válido *dia* valores variam de 1 a 31.
-   Válido *ano* os valores são 00 a 99 ou 1980 a 2099. Se você usar dois dígitos, os valores de 80 a 99 correspondem dos anos 1980 por meio de 1999.

## <a name="BKMK_examples"></a>Exemplos

Se as extensões de comando estiverem habilitadas, para exibir a data atual do sistema, digite:
```
date /t
```
Para alterar a data atual do sistema para 3 de agosto de 2007, você pode digitar qualquer uma das seguintes:
```
date 08.03.2007
date 08-03-07
date 8/3/07
```
Para exibir a data atual do sistema, seguida por uma solicitação para inserir uma nova data, digite:
```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yy)
```
Para manter a data atual e retornar ao prompt de comando, pressione ENTER. Para alterar a data atual, digite a nova data e, em seguida, pressione ENTER.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)