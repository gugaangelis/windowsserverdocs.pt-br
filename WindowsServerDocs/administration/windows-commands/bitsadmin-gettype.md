---
title: bitsadmin gettype
description: Artigo de referência para o comando Bitsadmin GetType, que recupera o tipo de trabalho do trabalho especificado.
ms.topic: reference
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6edd13a6647852fd9491254864199895a07ce1f0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024390"
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

#### <a name="output"></a>Saída

Os valores de saída retornados podem ser:

| Tipo | Descrição |
| --------------- | ----------- |
| Baixar | O trabalho é um download. |
| Carregar | O trabalho é um upload. |
| Carregar-responder | O trabalho é um upload-resposta. |
| Unknown | O trabalho tem um tipo desconhecido. |

## <a name="examples"></a>Exemplos

Para recuperar o tipo de trabalho para o trabalho chamado *myDownloadJob*:

```
bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
