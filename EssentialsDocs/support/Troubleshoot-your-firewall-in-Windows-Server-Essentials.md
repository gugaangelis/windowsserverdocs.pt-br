---
title: Solucionar problemas de seu firewall no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 3c48d2abb7fd8431f40f76f8eece5c4142be4c75
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Solucionar problemas de seu firewall no Windows Server Essentials
 
>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Se você tiver problemas com o acesso remoto, execute o Assistente de acesso do reparo de qualquer lugar.  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>Para executar o Assistente de acesso do reparo de qualquer lugar  
  
1.  Abra o painel.  
  
2.  Clique em **configurações**, clique no **acesso em qualquer local** guia e, em seguida, clique em **reparo**.  
  
3.  Siga as instruções do Assistente de acesso do reparo de qualquer lugar.  
  
 Se você estiver usando uma configuração avançada de rede ou usando um firewall não são da Microsoft, talvez você precise abrir portas adicionais no firewall. As portas na tabela a seguir são registradas com Internet Assigned Numbers Authority (IANA).  
  
|Número da porta|Descrição|  
|-----------------|-----------------|  
|65500|Serviço web de certificado|  
|65510 e 65515|Site de implantação do computador cliente|  
|65520|Serviço Web para os computadores cliente Mac|  
|65532|Estrutura do provedor para comunicações de loopback de servidor|  
|6602|Estrutura do provedor para comunicação entre o servidor e computadores cliente|  
  
## <a name="see-also"></a>Consulte também  
  
-   [Use o acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o acesso via Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o acesso em qualquer lugar](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Suporte ao Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Suporte ao Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

