---
title: remover definição de Telnet
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 37c4d84d1664fdc13ea7ffec60bf981b264dba00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853887"
---
# <a name="telnet-unset"></a>telnet: unset

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desativa anteriormente o conjunto de opções.   
## <a name="syntax"></a>Sintaxe  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|bsasdel|Envia **Backspace** como um **Backspace**.|  
|CRLF|Envia o **Enter** chave como uma CR. Também conhecido como alimentação de linha modo.|  
|delasbs|Envia **exclua** como **excluir**.|  
|escape|Remove a configuração de caractere de escape.|  
|eco local|Desativa o eco local.|  
|logging|Desativa o registro em log.|  
|ntlm|Desativa a autenticação NTLM.|  
|?|Exibe a Ajuda para este comando.|  
## <a name="BKMK_Examples"></a>Exemplos  
Desative o registro.  
```  
u logging  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
