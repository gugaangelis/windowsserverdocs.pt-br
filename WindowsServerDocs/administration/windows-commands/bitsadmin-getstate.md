---
title: bitsadmin getstate
description: Tópico de referência para o comando GetState do Bitsadmin, que recupera o estado do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab014c96c6d5d62232243d704d41d33cfcfc50f0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717534"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate

Recupera o estado do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getstate <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

#### <a name="output"></a>Saída

Os valores de saída retornados podem ser:

| Estado | Descrição |
| --------------- | ----------- |
| Em fila | O trabalho está aguardando para ser executado. |
| Connecting | O BITS está entrando em contato com o servidor. |
| Transferindo | O BITS está transferindo dados. |
| Transferidos | O BITS transferiu com êxito todos os arquivos no trabalho. |
| Suspenso | O trabalho está em pausa. |
| Erro | Ocorreu um erro não recuperável; a transferência não será repetida. |
| Transient_Error | Ocorreu um erro recuperável; a transferência é repetida quando o atraso mínimo de repetição expira. |
| Confirmado | O trabalho foi concluído. |
| Canceled | O trabalho foi cancelado. |

## <a name="examples"></a>Exemplos

Para recuperar o estado do trabalho chamado *myDownloadJob*:

```
bitsadmin /getstate myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
