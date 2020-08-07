---
title: telnet open
description: Artigo de referência para o Telnet aberto, que se conecta a um servidor Telnet.
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11498b2d2f38b96725608e6e72d30d0d563fd88a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881702"
---
# <a name="telnet-open"></a>Telnet: abrir

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conecta-se a um servidor Telnet.

## <a name="syntax"></a>Sintaxe
```
o[pen] <hostname> [<Port>]
```
#### <a name="parameters"></a>Parâmetros

| Parâmetro  |                                        Descrição                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         Especifica o nome do computador ou o endereço IP.                         |
|  [<Port>]  | Especifica a porta TCP na qual o servidor Telnet está escutando. O padrão é a porta TCP 23. |

## <a name="examples"></a>Exemplos
Conecte-se a um servidor Telnet em telnet.microsoft.com.
```
o telnet.microsoft.com
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
