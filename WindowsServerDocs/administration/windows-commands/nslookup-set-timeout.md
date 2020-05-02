---
title: nslookup set timeout
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68e9630b9690c9b6c9d4c316f8b328897289362c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723539"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera o número inicial de segundos para aguardar uma resposta a uma solicitação de pesquisa.
## <a name="syntax"></a>Sintaxe
```
set timeout=<Number>
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                           Descrição                                            |
|-----------------|--------------------------------------------------------------------------------------------------|
|    <Number>     | Especifica o número de segundos de espera por uma resposta. O número padrão de segundos a aguardar é 5. |
| {ajuda &#124;?} |                      Exibe um breve resumo dos subcomandos **nslookup** .                       |

## <a name="remarks"></a>Comentários
- Quando uma resposta a uma solicitação não é recebida dentro do período de tempo especificado, o tempo limite é duplicado e a solicitação é enviada novamente. Você pode usar o comando **set Retry** para controlar o número de repetições.
  ## <a name="examples"></a>Exemplos
  Para definir o tempo limite para obter uma resposta para 2 segundos:
  ```
  set timeout=2
  ```
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup Set Retry](nslookup-set-retry.md)
