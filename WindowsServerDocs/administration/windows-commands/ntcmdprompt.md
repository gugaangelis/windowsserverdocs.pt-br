---
title: ntcmdprompt
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fef1641bf1b48bd1fe4aaf284ed309ab4d4d5f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372674"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Executa o interpretador de comando **cmd. exe**, em vez de **Command.com**, depois de executar um encerramento e permanecer residente (TSR) ou depois de iniciar o prompt de comando de dentro de um aplicativo do MS-dos.
## <a name="syntax"></a>Sintaxe
```
ntcmdprompt
```
### <a name="parameters"></a>Parâmetros

| Parâmetro |             Descrição              |
|-----------|--------------------------------------|
|    /?     | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários
- Quando o **Command.com** está em execução, alguns recursos do **cmd. exe**, como a exibição do **doskey** do histórico de comandos, não estão disponíveis. Se preferir executar o interpretador de comando **cmd. exe** depois de iniciar um encerramento e permanecer residente (TSR) ou iniciar o prompt de comando de dentro de um aplicativo baseado no MS-dos, você poderá usar o comando **ntcmdprompt** . No entanto, tenha em mente que o TSR pode não estar disponível para uso quando você estiver executando **cmd. exe**. Você pode incluir o comando **ntcmdprompt** no arquivo **config. NT** ou no arquivo de inicialização personalizado equivalente em um arquivo de informações de programa (PIF) do aplicativo.
  ## <a name="examples"></a>Exemplos
  Para incluir o **ntcmdprompt** em seu arquivo **config. NT** ou no arquivo de inicialização de configuração especificado no PIF, digite: **ntcmdprompt**
  ## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

