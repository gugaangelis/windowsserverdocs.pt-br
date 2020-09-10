---
title: bitsadmin getstate
description: Artigo de referência para o comando GetState do Bitsadmin, que recupera o estado do trabalho especificado.
ms.topic: reference
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2faf05bd245f0b59eb9f10d0d46d96e8e1d3e11c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631651"
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
| Erro do | Ocorreu um erro não recuperável; a transferência não será repetida. |
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
