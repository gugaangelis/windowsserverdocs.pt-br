---
title: Telnet aberto
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 186664a75978f589a9a26047c72b9db74dd2dc4d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441123"
---
# <a name="telnet-open"></a>Telnet: abrir

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Conecta-se a um servidor telnet.    
## <a name="syntax"></a>Sintaxe  
```  
o[pen] <hostname> [<Port>]  
```  
### <a name="parameters"></a>Parâmetros  

| Parâmetro  |                                        Descrição                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         Especifica o nome do computador ou endereço IP.                         |
|  [<Port>]  | Especifica a porta TCP que o servidor telnet está escutando. O padrão é a porta TCP 23. |

## <a name="BKMK_Examples"></a>Exemplos  
Conecte-se a um servidor de telnet em telnet.microsoft.com.  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
