---
title: desdefinição de Telnet
description: Tópico de comandos do Windows para a desdefinição de Telnet, que desativa as opções definidas anteriormente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34d52eedb2a5547ad0e3f2912dbe2a250eaf8fc5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833139"
---
# <a name="telnet-unset"></a>Telnet: remover definição

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desativa as opções definidas anteriormente.   

## <a name="syntax"></a>Sintaxe  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
#### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|bsasdel|Envia **Backspace** como um **Backspace**.|  
|CRLF|Envia a tecla **Enter** como uma CR. Também conhecido como modo de alimentação de linha.|  
|delasbs|Envia **delete** como **delete**.|  
|escape|Remove a configuração de caractere de escape.|  
|localecho|Desativa o localecho.|  
|log|Desativa o registro em log.|  
|NTLM|Desativa a autenticação NTLM.|  
|?|Exibe a ajuda para este comando.|  
## <a name="examples"></a><a name=BKMK_Examples></a>Disso  
Desative o registro em log.  
```  
u logging  
```  
## <a name="additional-references"></a>Referências adicionais  
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
