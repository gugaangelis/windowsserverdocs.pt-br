---
title: bitsadmin info
description: Tópico de comandos do Windows para **Bitsadmin info**, que exibe informações de resumo sobre o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9d8b840e39620db01c44001d9cb7d593be2be5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850339"
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

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera informações sobre o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)