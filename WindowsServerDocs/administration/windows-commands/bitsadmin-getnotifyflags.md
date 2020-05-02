---
title: bitsadmin getnotifyflags
description: Tópico de referência para o comando Bitsadmin getnotifyflags, que recupera os sinalizadores de notificação para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36e4c3584b2e3be9c9985756aeaec08b40e74b0c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717763"
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
