---
title: tempo limite
description: Artigo de referência para tempo limite, que pausa o processador de comando para o número de segundos especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62ead9473a9034c02fab18f2318ecb5162511922
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930088"
---
# <a name="timeout"></a>tempo limite

Pausa o processador de comandos para o número de segundos especificado.



## <a name="syntax"></a>Sintaxe

```
timeout /t <TimeoutInSeconds> [/nobreak]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/t\<TimeoutInSeconds>|Especifica o número decimal de segundos (entre-1 e 99999) a aguardar antes do processamento do processador de comando continuar. O valor-1 faz com que o computador aguarde indefinidamente por um pressionamento de tecla.|
|/nobreak|Especifica para ignorar os traços de tecla do usuário.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O comando **Timeout** normalmente é usado em arquivos em lotes.
-   Um pressionamento de tecla do usuário retoma a execução do processador de comando imediatamente, mesmo que o período de tempo limite não tenha expirado.
-   Quando usado em conjunto com o comando **Sleep** , o **tempo limite** é semelhante ao comando **Pause** .

## <a name="examples"></a>Exemplos

Para pausar o processador de comandos por dez segundos, digite:
```
timeout /t 10
```
Para pausar o processador de comando por 100 segundos e ignorar qualquer pressionamento de tecla, digite:
```
timeout /t 100 /nobreak
```
Para pausar o processador de comandos indefinidamente até que uma tecla seja pressionada, digite:
```
timeout /t -1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
