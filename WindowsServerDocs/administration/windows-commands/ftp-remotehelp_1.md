---
title: remotehelp FTP
description: Tópico de referência para o comando FTP remotehelp, que exibe a ajuda para comandos remotos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 659de5487890b50aab9004f650e4584085e7306c
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820316"
---
# <a name="ftp-remotehelp"></a>remotehelp FTP

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

- [aspas de FTP](ftp-quote.md)

- [literal de FTP](ftp-literal_1.md)

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
