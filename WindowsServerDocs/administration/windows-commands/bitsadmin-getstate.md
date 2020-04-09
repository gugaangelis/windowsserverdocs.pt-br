---
title: bitsadmin getstate
description: Tópico de comandos do Windows para **Bitsadmin GetState**, que recupera o estado do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43cd8c8e614cce65f55b16fc5395b1d37de0cf95
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850459"
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

## <a name="output"></a>Saída

Os valores de saída incluem:

| Estado | Descrição |
| --------------- | ----------- |
| Em fila | O trabalho está aguardando para ser executado. |
| Conectando | O BITS está entrando em contato com o servidor. |
| Transferir | O BITS está transferindo dados. |
| Transferidos | O BITS transferiu com êxito todos os arquivos no trabalho. |
| Suspenso | O trabalho está em pausa. |
| Error | Ocorreu um erro não recuperável; a transferência não será repetida. |
| Transient_Error | Ocorreu um erro recuperável; a transferência é repetida quando o atraso mínimo de repetição expira. |
| Confirmada | O trabalho foi concluído. |
| Cancelado | O trabalho foi cancelado. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera o estado do trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getstate myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
