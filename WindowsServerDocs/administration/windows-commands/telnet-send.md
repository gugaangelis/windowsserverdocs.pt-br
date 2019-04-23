---
title: envio de Telnet
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32345b22395107f4a2c3d88894126d4e5e0875a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842357"
---
# <a name="telnet-send"></a>telnet: send

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia comandos telnet para o servidor telnet.   
## <a name="syntax"></a>Sintaxe  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|sol|Envia o comando do telnet Abort Output.|  
|ayt|Envia o comando do telnet são você lá.|  
|brk|Envia o comando telnet brk.|  
|esc|Envia o caractere de escape telnet atual.|  
|ip|Envia o processo de interrupção de comando do telnet.|  
|sincronização|Envia a sincronização de comando do telnet.|  
|<string>|Envia a você digitar qualquer cadeia de caracteres para o servidor telnet.|  
|?|Exibe a Ajuda associada a este comando.|  
## <a name="BKMK_Examples"></a>Exemplos  
Send tem existe para o servidor telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
