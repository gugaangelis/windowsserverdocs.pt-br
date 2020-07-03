---
title: telnet open
description: Artigo de referência para o Telnet aberto, que se conecta a um servidor Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3b27088d464e62b24479eaa87f44b91f7d95d12
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935335"
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
