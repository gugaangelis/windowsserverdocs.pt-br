---
title: timeout
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3997399b732c494050797c83a0a52938574986bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830167"
---
# <a name="timeout"></a>timeout



Pausa o processador de comando para o número de segundos especificado.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/t \<TimeoutInSeconds>|Especifica o número decimal de segundos (entre -1 e 99999) de espera antes que o processador de comando continua o processamento. O valor -1 faz com que o computador Aguarde indefinidamente por um pressionamento de tecla.|
|/nobreak|Especifica para ignorar os pressionamentos de tecla do usuário.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O **timeout** comando normalmente é usado em arquivos em lotes.
-   Um pressionamento de tecla do usuário retoma a execução de processador de comando imediatamente, mesmo que não tenha expirado o período de tempo limite.
-   Quando usado em conjunto com o **suspensão** comando, **timeout** é semelhante ao **pausar** comando.

## <a name="BKMK_examples"></a>Exemplos

Para pausar o processador de comandos por dez segundos, digite:
```
timeout /t 10
```
Para pausar o processador de comandos por 100 segundos e ignorar qualquer pressionamento de tecla, digite:
```
timeout /t 100 /nobreak
```
Para pausar o processador de comando indefinidamente até que uma tecla é pressionada, digite:
```
timeout /t -1
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
