---
title: envio de FTP
description: Tópico de referência para o comando FTP Send, que copia um arquivo local para o computador remoto usando o tipo de transferência de arquivo atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12ce45a0eb26e1aa4a0d7daace831751e1b67f4a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820296"
---
# <a name="ftp-send"></a>envio de FTP

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo local para o computador remoto usando o tipo de transferência de arquivo atual.

> [!NOTE]
> Esse comando é o mesmo que o [comando FTP Put](ftp-put.md).

## <a name="syntax"></a>Sintaxe

```
send <localfile> [<remotefile>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<localfile>` | Especifica o arquivo local a ser copiado. |
| `<remotefile>` | Especifica o nome a ser usado no computador remoto. Se você não especificar um *remotefile*, o arquivo receberá o nome de *LocalFile* . |

### <a name="examples"></a>Exemplos

Para copiar o arquivo local *Test. txt* e nomeá-lo de *Test1. txt* no computador remoto, digite:

```
send test.txt test1.txt
```

Para copiar o arquivo de *programa. exe* local para o computador remoto, digite:

```
send program.exe
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
