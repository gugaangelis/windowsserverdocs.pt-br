---
title: exit
description: Artigo de referência para Exit, que sai do interpretador de comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3cee4a2-6210-46f0-b8e4-7381c3c4e530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d28f15ba1453b32d8e464fd768a3b7895819d11c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931454"
---
# <a name="exit"></a>exit

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sai do interpretador de comando ou do script do lote atual.

## <a name="syntax"></a>Sintaxe

```
exit [/b] [<exitcode>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /b | Sai do script do lote atual em vez de sair do Cmd.exe. Se executado de fora de um script em lote, sairá Cmd.exe. |
| `<exitcode>` | Especifica um número numérico. Se **/b** for especificado, a variável de ambiente ERRORLEVEL será definida para esse número. Se você estiver encerrando o interpretador de comandos, o código de saída do processo será definido como esse número. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para fechar o interpretador de comando, digite:

```
exit
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
