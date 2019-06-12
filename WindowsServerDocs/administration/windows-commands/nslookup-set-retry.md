---
title: nslookup set retry
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 876d8332e778aa0b3049354a21fbe01adb883729
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436655"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define o número de repetições.
## <a name="syntax"></a>Sintaxe
```
set retry=<Number>
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                      Descrição                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | Especifica o novo valor para o número de repetições. O número de repetições padrão é 4. |
| {Ajuda &#124; ?} |                 Exibe um resumo breve dos **nslookup** subcomandos.                  |

## <a name="remarks"></a>Comentários
- Quando uma resposta a uma solicitação não for recebida dentro de um determinado período de tempo, o período de tempo limite é duplicado e a solicitação é enviada novamente. O valor de repetição controla quantas vezes uma solicitação é enviada novamente antes de desistir. Você pode alterar o período de tempo limite com o **definir tempo limite** subcomando.
  ## <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup definir tempo limite](nslookup-set-timeout.md)
