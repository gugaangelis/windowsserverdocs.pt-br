---
title: bitsadmin getnotifyflags
description: Tópico de comandos do Windows para **getnotifyflags bitsadmin** -recupera os sinalizadores de notificação para o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 690e94805c5e61d96603e4ade102fb3a4bda409e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889277"
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
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

O trabalho pode conter um ou mais dos seguintes sinalizadores de notificação.

|---|---| | 0x001 | Gerar um evento quando o trabalho de todos os arquivos foram transferidos. | | 0x002 | Gerar um evento quando ocorre um erro. | | 0x004 | Desabilitar as notificações. | | 0x008 | Gerar um evento quando o trabalho é modificado ou o progresso de transferência é feito. |

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera os sinalizadores de notificação do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)