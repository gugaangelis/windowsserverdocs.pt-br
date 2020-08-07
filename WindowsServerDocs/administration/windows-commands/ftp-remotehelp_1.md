---
title: ftp remotehelp
description: Artigo de referência para o comando FTP remotehelp, que exibe a ajuda para comandos remotos.
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e428a43ddcf36f1125c3fa83ddeeeff3cae979d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889019"
---
# <a name="ftp-remotehelp"></a>ftp remotehelp

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe a ajuda para comandos remotos.

## <a name="syntax"></a>Sintaxe

```
remotehelp [<command>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| `[<command>]` | Especifica o nome do comando sobre o qual você deseja obter ajuda. Se `<command>` não for especificado, esse comando exibirá uma lista de todos os comandos remotos. Você também pode executar comandos remotos usando [aspas de FTP](ftp-quote.md) ou o literal de [FTP](ftp-literal_1.md). |

### <a name="examples"></a>Exemplos

Para exibir uma lista de comandos remotos, digite:

```
remotehelp
```

Para exibir a *sintaxe do comando remoto feito* , digite:

```
remotehelp feat
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [ftp quote](ftp-quote.md)

- [ftp literal](ftp-literal_1.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
