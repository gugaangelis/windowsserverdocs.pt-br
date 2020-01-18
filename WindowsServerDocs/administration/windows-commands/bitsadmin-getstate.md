---
title: bitsadmin getstate
description: Tópico de comandos do Windows para Bitsadmin GetState
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
ms.openlocfilehash: 2cff790c8787b1514e8523a4583184d6f6a59efc
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259101"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate


Recupera o estado do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
|    Job    | O nome de exibição ou o GUID do trabalho |

## <a name="remarks"></a>Comentários

Os possíveis estados são:

|      Estado      | Descrição |
| --------------- | ----------- |
| EM fila          | O trabalho está aguardando para ser executado. |
| CONECTANDO      | O BITS está entrando em contato com o servidor. |
| TRANSFERIR    | O BITS está transferindo dados. |
| TRANSFERIDOS     | O BITS transferiu com êxito todos os arquivos no trabalho. |
| SUSPENSO       | O trabalho está em pausa. |
| ERRO           | Ocorreu um erro não recuperável; a transferência não será repetida. |
| TRANSIENT_ERROR | Ocorreu um erro recuperável; a transferência é repetida quando o atraso mínimo de repetição expira. |
| CONFIRMADO    | O trabalho foi concluído. |
| Cancel        | O trabalho foi cancelado. |

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera o estado do trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
