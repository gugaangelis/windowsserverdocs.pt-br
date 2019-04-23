---
title: bitsadmin getstate
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f7ed7529fda264efaceb6b4b36e36e728c211f3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889617"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate



Recupera o estado do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Os possíveis estados são:

|-----|-----| | NA FILA | O trabalho está esperando para ser executado. | | CONECTAR-SE | BITS é entrar em contato com o servidor. | | TRANSFERINDO | BITS estiver transferindo dados. | | SUSPENSO | O trabalho está em pausa. | | ERRO | Ocorreu um erro não recuperável; a transferência não será repetida. | | TRANSIENT_ERROR | Ocorreu um erro recuperável; a transferência tentará novamente quando o intervalo de repetição mínimo expira. | | CONFIRMADO | O trabalho foi concluído. | | CANCELADO | O trabalho foi cancelado. |

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera o estado do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)