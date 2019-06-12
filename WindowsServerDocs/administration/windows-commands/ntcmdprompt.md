---
title: ntcmdprompt
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 583f56c294e66542a75efca09e97d57ae54a8cea
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436419"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Executa o interpretador de comandos **Cmd.exe**, em vez de **Command.com**, depois de executar um encerramento e fique residente TSR () ou depois de iniciar o prompt de comando dentro de um aplicativo do MS-DOS.
## <a name="syntax"></a>Sintaxe
```
ntcmdprompt
```
### <a name="parameters"></a>Parâmetros

| Parâmetro |             Descrição              |
|-----------|--------------------------------------|
|    /?     | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários
- Quando **Command.com** estiver em execução, alguns recursos do **Cmd.exe**, como o **doskey** a exibição de histórico de comandos, não estão disponíveis. Se você preferir executar o **Cmd.exe** interpretador de comando depois de você ter iniciado um encerramento e fique residente TSR () ou o prompt de comando dentro de um aplicativo baseado no MS-DOS, você pode usar o **ntcmdprompt**  comando. No entanto, tenha em mente que o TSR pode não estar disponível para uso quando você estiver executando **Cmd.exe**. Você pode incluir a **ntcmdprompt** no seu **config** arquivo ou o arquivo de inicialização personalizado equivalente no arquivo de informações do programa do aplicativo (Pif).
  ## <a name="examples"></a>Exemplos
  Para incluir **ntcmdprompt** no seu **config** arquivo ou o arquivo de inicialização de configuração especificado no Pif, tipo: **ntcmdprompt**
  ## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

