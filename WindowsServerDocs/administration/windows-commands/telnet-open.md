---
title: Telnet aberto
description: Tópico de referência para o Telnet aberto, que se conecta a um servidor Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0be8aca4cb061df55674a405eb1cf2a5e643cb4f
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222713"
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
