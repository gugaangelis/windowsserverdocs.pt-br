---
title: Telnet aberto
description: O tópico comandos do Windows para telnet aberto, que se conecta a um servidor Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b100d2b53a340a083f22d4fd88c42363642d5da5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833329"
---
# <a name="telnet-open"></a>Telnet: abrir

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Conecte-se a um servidor Telnet em telnet.microsoft.com.  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
