---
title: ntcmdprompt
description: Artigo de referência para o comando ntcmdprompt, que executa o interpretador de comando **Cmd.exe**, em vez de **Command.com**, depois de executar um encerramento e permanecer residente (TSR) ou depois de iniciar o prompt de comando de dentro de um aplicativo MS-dos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a70a8cfdbe6e37bfb8d90bed23e961d100843506
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925347"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Executa o interpretador de comando **Cmd.exe**, em vez de **Command.com**, depois de executar um encerramento e permanecer residente (TSR) ou depois de iniciar o prompt de comando de dentro de um aplicativo MS-dos.

## <a name="syntax"></a>Sintaxe

```
ntcmdprompt
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Quando **Command.com** está em execução, alguns recursos de **Cmd.exe**, como a exibição do **doskey** do histórico de comandos, não estão disponíveis. Se preferir executar o interpretador de comando **Cmd.exe** depois de iniciar um encerramento e permanecer residente (TSR) ou iniciar o prompt de comando de dentro de um aplicativo baseado no MS-dos, você poderá usar o comando **ntcmdprompt** . No entanto, tenha em mente que o TSR pode não estar disponível para uso quando você estiver executando o **Cmd.exe**. Você pode incluir o comando **ntcmdprompt** no arquivo **config. NT** ou no arquivo de inicialização personalizado equivalente em um arquivo de informações de programa (PIF) do aplicativo.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
