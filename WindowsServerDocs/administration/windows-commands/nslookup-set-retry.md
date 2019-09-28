---
title: nslookup set retry
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 306bcc4f5e7ac98767c3c2e274100cf917874a8e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372862"
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
|    <Number>     | Especifica o novo valor para o número de repetições. O número padrão de repetições é 4. |
| {ajuda &#124; ?} |                 Exibe um breve resumo dos subcomandos **nslookup** .                  |

## <a name="remarks"></a>Comentários
- Quando uma resposta a uma solicitação não é recebida dentro de um determinado período de tempo, o período de tempo limite é duplicado e a solicitação é reenviada. O valor de repetição controla quantas vezes uma solicitação é enviada novamente antes de desistir. Você pode alterar o período de tempo limite com o subcomando **set timeout** .
  ## <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup set timeout](nslookup-set-timeout.md)
