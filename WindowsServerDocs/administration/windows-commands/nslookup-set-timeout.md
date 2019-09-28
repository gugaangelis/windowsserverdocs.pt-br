---
title: nslookup set timeout
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32fcfcaeccb6599e9aaca21f9c085bb00857479c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372757"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o número inicial de segundos para aguardar uma resposta a uma solicitação de pesquisa.
## <a name="syntax"></a>Sintaxe
```
set timeout=<Number>
```
## <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                           Descrição                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Especifica o número de segundos de espera por uma resposta. O número padrão de segundos a aguardar é 5. |
| {ajuda &#124; ?} |                      Exibe um breve resumo dos subcomandos **nslookup** .                       |

## <a name="remarks"></a>Comentários
- Quando uma resposta a uma solicitação não é recebida dentro do período de tempo especificado, o tempo limite é duplicado e a solicitação é enviada novamente. Você pode usar o comando **set Retry** para controlar o número de repetições.
  ## <a name="BKMK_examples"></a>Disso
  O exemplo a seguir define o tempo limite para obter uma resposta para 2 segundos:
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup Set Retry](nslookup-set-retry.md)
