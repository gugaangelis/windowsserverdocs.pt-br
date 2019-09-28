---
title: date
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d7328b2b5d3c78fdfd741918d76e26195f0af4a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378821"
---
# <a name="date"></a>date



Exibe ou define a data do sistema. Se usado sem parâmetros, **Data** exibe a configuração de data atual do sistema e solicita que você insira uma nova data.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
date [/t | <Month-Day-Year>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Month-dia-ano >|Define a data especificada, em que *mês* é o mês (um ou dois dígitos), *dia* é o dia (um ou dois dígitos) e *ano* é o ano (dois ou quatro dígitos).|
|/t|Exibe a data atual sem solicitar uma nova data.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para alterar a data atual, você deve ter credenciais administrativas.
-   Você deve separar valores para *mês*, *dia*e *ano* com pontos (.), hifens (-) ou barras (/).
-   Os valores de *mês* válidos são de 1 a 12.
-   Os valores de *dia* válidos são de 1 a 31.
-   Valores de *ano* válidos são 00 a 99 ou 1980 a 2099. Se você usar dois dígitos, os valores de 80 a 99 corresponderão aos anos de 1980 a 1999.

## <a name="BKMK_examples"></a>Disso

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
Enter the new date: (mm-dd-yy)
```
Para manter a data atual e retornar ao prompt de comando, pressione ENTER. Para alterar a data atual, digite a nova data e pressione ENTER.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)