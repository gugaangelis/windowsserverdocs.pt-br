---
title: nslookup set retry
description: Artigo de referência para o comando Set Retry do nslookup, que define o número de tentativas de obter informações de um servidor especificado.
ms.topic: reference
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fa8b588c49b61c2269325b1b9e67d350f837c5ad
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637520"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se uma resposta não for recebida dentro de um determinado período de tempo, o período de tempo limite será duplicado e a solicitação será enviada novamente. Esse comando define o número de vezes que uma solicitação é enviada novamente a um servidor para obter informações, antes de desistir.

> [!NOTE]
> Para alterar o período de tempo antes que a solicitação expire, use o comando [nslookup set timeout](nslookup-set-timeout.md) .

## <a name="syntax"></a>Sintaxe

```
set retry=<number>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ---------- | ---------- |
| `<number>` | Especifica o novo valor para o número de repetições. O número padrão de repetições é **4**. |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [nslookup set timeout](nslookup-set-timeout.md)
