---
title: tempo limite
description: Artigo de referência para o comando timeout, que pausa o processador de comando para o número de segundos especificado.
ms.topic: reference
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: edb4d14614099b0e501df175e619285928344c78
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156632"
---
# <a name="timeout"></a>tempo limite

Pausa o processador de comandos para o número de segundos especificado. Esse comando é normalmente usado em arquivos em lotes.

## <a name="syntax"></a>Sintaxe

```
timeout /t <timeoutinseconds> [/nobreak]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /t `<timeoutinseconds>` | Especifica o número decimal de segundos (entre-1 e 99999) a aguardar antes do processamento do processador de comando continuar. O valor **-1** faz com que o computador aguarde indefinidamente por um pressionamento de tecla. |
| /nobreak | Especifica para ignorar os traços de tecla do usuário. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Um pressionamento de tecla do usuário retoma a execução do processador de comando imediatamente, mesmo que o período de tempo limite não tenha expirado.

- Quando usado em conjunto com a ferramenta de **suspensão** do kit de recursos, o **tempo limite** é semelhante ao comando **Pause** .

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
