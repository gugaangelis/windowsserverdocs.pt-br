---
title: bitsadmin gettype
description: O tópico de comandos do Windows para **Bitsadmin GetType**, que recupera o tipo de trabalho do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 66a1fc5b0478e1eec26557dc9a7f76d50abcb8b6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850439"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype

Recupera o tipo de trabalho do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /gettype <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="output"></a>Saída

Os valores de saída incluem:

| Tipo | Descrição |
| --------------- | ----------- |
| {1&gt;{2&gt;Baixar&lt;2}&lt;1} | O trabalho é um download. |
| Carregar | O trabalho é um upload. |
| Carregar-responder | O trabalho é um upload-resposta. |
| Desconhecido | O trabalho tem um tipo desconhecido. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera o tipo de trabalho para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)