---
title: nslookup set retry
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 615fdfa2-fa29-47a8-8c9e-a6c5b45b3b71
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b95c4c8af2d7960270fd43f7a766b313ddbc07a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838339"
---
# <a name="nslookup-set-retry"></a>nslookup set retry

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define o número de repetições.
## <a name="syntax"></a>Sintaxe
```
set retry=<Number>
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                      Descrição                                       |
|-----------------|----------------------------------------------------------------------------------------|
|    <Number>     | Especifica o novo valor para o número de repetições. O número padrão de repetições é 4. |
| {ajuda &#124; ?} |                 Exibe um breve resumo dos subcomandos **nslookup** .                  |

## <a name="remarks"></a>Comentários
- Quando uma resposta a uma solicitação não é recebida dentro de um determinado período de tempo, o período de tempo limite é duplicado e a solicitação é reenviada. O valor de repetição controla quantas vezes uma solicitação é enviada novamente antes de desistir. Você pode alterar o período de tempo limite com o subcomando **set timeout** .
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup set timeout](nslookup-set-timeout.md)
