---
title: ftp open
description: Artigo de referência para o comando FTP Open, que se conecta ao servidor FTP especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 888b8f917d82d72f47a737c2f0edc42451de7164
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925792"
---
# <a name="ftp-open"></a>ftp open

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conecta-se ao servidor FTP especificado.

## <a name="syntax"></a>Sintaxe

```
open <computer> [<port>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<computer>` | Especifica o computador remoto ao qual você está tentando se conectar. Você pode usar um endereço IP ou nome do computador (nesse caso, um servidor DNS ou um arquivo hosts deve estar disponível). |
| `[<port>]` | Especifica um número de porta TCP a ser usado para se conectar a um servidor FTP. Por padrão, a porta TCP 21 é usada. |

### <a name="examples"></a>Exemplos

Para se conectar ao servidor FTP em *ftp.Microsoft.com*, digite:

```
open ftp.microsoft.com
```

Para se conectar ao servidor FTP em *ftp.Microsoft.com* que está escutando na porta TCP *755*, digite:

```
open ftp.microsoft.com 755
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
