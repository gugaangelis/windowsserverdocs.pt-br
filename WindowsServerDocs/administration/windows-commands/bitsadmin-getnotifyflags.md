---
title: bitsadmin getnotifyflags
description: Tópico de comandos do Windows para **Bitsadmin getnotifyflags**, que recupera os sinalizadores de notificação para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3138baea05f793cfb587d3f8fb669d446daea6b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850579"
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

| Flag | Descrição |
| ----- | ----- |
| 0x001 | Gerar um evento quando todos os arquivos no trabalho forem transferidos. |
| 0x002 | Gerar um evento quando ocorrer um erro. |
| 0x004 | Desabilitar notificações. |
| 0x008 | Gerar um evento quando o trabalho for modificado ou o progresso da transferência for feito. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera os sinalizadores de notificação para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getnotifyflags myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)