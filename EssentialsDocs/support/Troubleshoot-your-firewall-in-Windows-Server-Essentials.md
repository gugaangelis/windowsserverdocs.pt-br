---
title: Solucionar problemas do firewall no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 51d94b67-8b9b-4159-80dd-f652d73a43cb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ff27915f3d6c92416ed22b7e143bdc1cf3b385f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853179"
---
# <a name="troubleshoot-your-firewall-in-windows-server-essentials"></a>Solucionar problemas do firewall no Windows Server Essentials
 
>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Se você tiver problemas com o acesso remoto, execute o assistente Reparar Acesso em Qualquer Local.  
  
### <a name="to-run-the-repair-anywhere-access-wizard"></a>Para executar o assistente Reparar Acesso em Qualquer Local  
  
1. Abra o Dashboard.  
  
2. Clique em **Configurações**, clique na guia **Acesso em Qualquer Local** e clique em **Reparar**.  
  
3. Siga as instruções no assistente Reparar Acesso em Qualquer Local.  
  
   Se você estiver usando uma configuração de rede avançada ou um firewall não Microsoft, talvez seja necessário abrir portas adicionais no firewall. As portas na tabela a seguir são registradas na IANA (Internet Assigned Numbers Authority).  
  
|Número da Porta|Descrição|  
|-----------------|-----------------|  
|65500|Serviço Web de certificado|  
|65510 e 65515|Site de implantação do computador cliente|  
|65520|Serviço Web para computadores cliente Mac|  
|65532|Estrutura do provedor para comunicações loopback do servidor|  
|6602|Estrutura do provedor para a comunicação entre os computadores cliente e servidor|  
  
## <a name="see-also"></a>Consulte também  
  
-   [Usar Acesso via Web remotos](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar Acesso via Web remotos](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar acesso em qualquer local](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Suporte ao Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Suporte ao Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

