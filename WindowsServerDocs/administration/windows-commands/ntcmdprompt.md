---
title: ntcmdprompt
description: Artigo de referência para o comando ntcmdprompt, que executa o interpretador de comando **Cmd.exe**, em vez de **Command.com**, depois de executar um encerramento e permanecer residente (TSR) ou depois de iniciar o prompt de comando de dentro de um aplicativo MS-dos.
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 53ed39f0fd455314dc5ccd90f0286f6dfd35ea72
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885307"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Executa o interpretador de comando **Cmd.exe**, em vez de **Command.com**, depois de executar um encerramento e permanecer residente (TSR) ou depois de iniciar o prompt de comando de dentro de um aplicativo MS-dos.

## <a name="syntax"></a>Sintaxe

```
ntcmdprompt
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | DESCRIÇÃO |
| --------- | ----------- |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Quando **Command.com** está em execução, alguns recursos de **Cmd.exe**, como a exibição do **doskey** do histórico de comandos, não estão disponíveis. Se preferir executar o interpretador de comando **Cmd.exe** depois de iniciar um encerramento e permanecer residente (TSR) ou iniciar o prompt de comando de dentro de um aplicativo baseado no MS-dos, você poderá usar o comando **ntcmdprompt** . No entanto, tenha em mente que o TSR pode não estar disponível para uso quando você estiver executando o **Cmd.exe**. Você pode incluir o comando **ntcmdprompt** no arquivo **config. NT** ou no arquivo de inicialização personalizado equivalente em um arquivo de informações de programa (PIF) do aplicativo.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
