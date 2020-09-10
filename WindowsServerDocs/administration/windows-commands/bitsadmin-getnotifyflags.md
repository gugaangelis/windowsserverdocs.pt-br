---
title: bitsadmin getnotifyflags
description: Artigo de referência para o comando Bitsadmin getnotifyflags, que recupera os sinalizadores de notificação para o trabalho especificado.
ms.topic: reference
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b7cdc2d65316bfcc856bb0fe55a9379d4fc6ba6b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631901"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags

Recupera os sinalizadores de notificação para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getnotifyflags <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="remarks"></a>Comentários

O trabalho pode conter um ou mais dos seguintes sinalizadores de notificação:

| Sinalizador | Descrição |
| ----- | ----- |
| 0x001 | Gerar um evento quando todos os arquivos no trabalho forem transferidos. |
| 0x002 | Gerar um evento quando ocorrer um erro. |
| 0x004 | Desabilitar notificações. |
| 0x008 | Gerar um evento quando o trabalho for modificado ou o progresso da transferência for feito. |

## <a name="examples"></a>Exemplos

Para recuperar os sinalizadores de notificação para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getnotifyflags myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
