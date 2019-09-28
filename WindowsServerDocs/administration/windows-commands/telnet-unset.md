---
title: desdefinição de Telnet
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6f0eb98c4168d2f664780dad42ca1aea5463d24
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383477"
---
# <a name="telnet-unset"></a>Telnet: remover definição

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desativa as opções definidas anteriormente.   
## <a name="syntax"></a>Sintaxe  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|bsasdel|Envia **Backspace** como um **Backspace**.|  
|CRLF|Envia a tecla **Enter** como uma CR. Também conhecido como modo de alimentação de linha.|  
|delasbs|Envia **delete** como **delete**.|  
|Fuga|Remove a configuração de caractere de escape.|  
|localecho|Desativa o localecho.|  
|logging|Desativa o registro em log.|  
|NTLM|Desativa a autenticação NTLM.|  
|?|Exibe a ajuda para este comando.|  
## <a name="BKMK_Examples"></a>Disso  
Desative o registro em log.  
```  
u logging  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
