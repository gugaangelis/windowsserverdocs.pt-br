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
ms.openlocfilehash: 36dc7f861e88cf991af57dda2f150107c6870f0f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441044"
---
# <a name="telnet-send"></a>telnet: send

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia comandos telnet para o servidor telnet.   
## <a name="syntax"></a>Sintaxe  
```  
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]  
```  
### <a name="parameters"></a>Parâmetros  

| Parâmetro |                     Descrição                      |
|-----------|------------------------------------------------------|
|    sol     |       Envia o comando do telnet Abort Output.        |
|    ayt    |       Envia o comando do telnet são você lá.       |
|    brk    |            Envia o comando telnet brk.            |
|    esc    |      Envia o caractere de escape telnet atual.      |
|    ip     |     Envia o processo de interrupção de comando do telnet.     |
|   sincronização   |           Envia a sincronização de comando do telnet.           |
| <string>  | Envia a você digitar qualquer cadeia de caracteres para o servidor telnet. |
|     ?     |     Exibe a Ajuda associada a este comando.      |

## <a name="BKMK_Examples"></a>Exemplos  
Send tem existe para o servidor telnet.  
```  
sen ayt  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
