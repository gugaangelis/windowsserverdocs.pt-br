---
title: bitsadmin gettype
description: Artigo de referência para o comando Bitsadmin GetType, que recupera o tipo de trabalho do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7224add1b503d9ec50e84879a47442c12447e5d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926642"
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

| Type | Description |
| --------------- | ----------- |
| Baixar | O trabalho é um download. |
| Carregar | O trabalho é um upload. |
| Carregar-responder | O trabalho é um upload-resposta. |
| Unknown (desconhecido) | O trabalho tem um tipo desconhecido. |

## <a name="examples"></a>Exemplos

Para recuperar o tipo de trabalho para o trabalho chamado *myDownloadJob*:

```
bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
