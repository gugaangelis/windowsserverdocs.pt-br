---
title: bitsadmin getstate
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 55be37a6b1b44b81ed9002e5e3b9eb1fd46bd0dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381227"
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
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Os possíveis Estados são:

|-----|-----| | NA fila | O trabalho está aguardando para ser executado. | | CONECTAndo | O BITS está entrando em contato com o servidor. | | Transferindo | O BITS está transferindo dados. | | SUSPENSO | O trabalho está em pausa. | | ERRO | Ocorreu um erro não recuperável; a transferência não será repetida. | | TRANSIENT_ERROR | Ocorreu um erro recuperável; a transferência é repetida quando o atraso mínimo de repetição expira. | | CONFIRMADO | O trabalho foi concluído. | | Cancelado | O trabalho foi cancelado. |

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera o estado do trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)