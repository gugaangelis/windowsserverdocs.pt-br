---
title: bitsadmin getnotifyflags
description: O tópico de comandos do Windows para **Bitsadmin getnotifyflags** -recupera os sinalizadores de notificação para o trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 56ee3a30050b6cc934b35bab24e9508911ea250e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381482"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags



Recupera os sinalizadores de notificação para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetNotifyFlags <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

O trabalho pode conter um ou mais dos seguintes sinalizadores de notificação.

|-----|-----| | 0x001 | Gerar um evento quando todos os arquivos no trabalho forem transferidos. | | 0x002 | Gerar um evento quando ocorrer um erro. | | 0x004 | Desabilitar notificações. | | 0x008 | Gerar um evento quando o trabalho for modificado ou o progresso da transferência for feito. |

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera os sinalizadores de notificação para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)