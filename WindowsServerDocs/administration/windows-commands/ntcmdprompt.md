---
title: ntcmdprompt
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5b0dcbc0735f20b63cbf441f4014411acd3a48e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723471"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Executa o interpretador de comando **cmd. exe**, em vez de **Command.com**, depois de executar um encerramento e permanecer residente (TSR) ou depois de iniciar o prompt de comando de dentro de um aplicativo do MS-dos.
## <a name="syntax"></a>Sintaxe
```
ntcmdprompt
```
#### <a name="parameters"></a>Parâmetros

| Parâmetro |             Descrição              |
|-----------|--------------------------------------|
|    /?     | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários
- Quando o **Command.com** está em execução, alguns recursos do **cmd. exe**, como a exibição do **doskey** do histórico de comandos, não estão disponíveis. Se preferir executar o interpretador de comando **cmd. exe** depois de iniciar um encerramento e permanecer residente (TSR) ou iniciar o prompt de comando de dentro de um aplicativo baseado no MS-dos, você poderá usar o comando **ntcmdprompt** . No entanto, tenha em mente que o TSR pode não estar disponível para uso quando você estiver executando **cmd. exe**. Você pode incluir o comando **ntcmdprompt** no arquivo **config. NT** ou no arquivo de inicialização personalizado equivalente em um arquivo de informações de programa (PIF) do aplicativo.
  ## <a name="examples"></a>Exemplos
  Para incluir o **ntcmdprompt** em seu arquivo **config. NT** ou no arquivo de inicialização de configuração especificado no PIF, digite: **ntcmdprompt**
  ## <a name="additional-references"></a>Referências adicionais
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

