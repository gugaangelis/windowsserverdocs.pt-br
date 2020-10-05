---
title: telnet open
description: Artigo de referência do comando telnet Open, que se conecta a um servidor Telnet.
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6db7f428f4b8c85c6e953a8fe4a9328b965898f8
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718003"
---
# <a name="telnet-open"></a>Telnet: abrir

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conecta-se a um servidor Telnet.

## <a name="syntax"></a>Sintaxe

```
o[pen] <hostname> [<port>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<hostname>` | Especifica o nome do computador ou o endereço IP. |
| `[<port>]` | Especifica a porta TCP na qual o servidor Telnet está escutando. O padrão é a porta TCP 23. |

## <a name="examples"></a>Exemplos

Para se conectar a um servidor Telnet em *Telnet.Microsoft.com*, digite:

```
o telnet.microsoft.com
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
