---
title: Solucionar problemas de conectividade de acesso via Web remoto no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3642575-b3ee-4488-b654-5bf9d3b8c935
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: af4725fd3b1861c847434e3ed62c3da030689fb5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-remote-web-access-connectivity-in-windows-server-essentials"></a>Solucionar problemas de conectividade de acesso via Web remoto no Windows Server Essentials
 
>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Normalmente, o Windows Server Essentials automaticamente pode configurar um roteador banda larga se o roteador for um UPnP certificado do dispositivo e se a configuração UPnP esteja habilitada no roteador.  
  
## <a name="possible-issues"></a>Possíveis problemas  
 Você pode enfrentar os seguintes problemas com conectividade de acesso via Web remoto:  
  
-   O roteador não está ativado ou não estiver conectado à rede.  
  
-   A configuração de UPnP do roteador está desativada.  
  
-   O roteador pode não suportar totalmente o padrão de UPnP. A Microsoft mantém uma lista dos roteadores que funcionam com os sistemas operacionais Windows. Para ver a lista dos roteadores (incluindo roteadores sem fio) que são compatíveis com o Windows Server Essentials, visite o [Centro de compatibilidade do Windows](https://www.microsoft.com/windows/compatibility/CompatCenter/Home).  
  
## <a name="possible-fixes"></a>Possíveis correções  
 As seguintes ações podem corrigir esses problemas:  
  
-   Verifique se o roteador está ligado e funcionando corretamente.  
  
-   Certifique-se de que o servidor está conectado diretamente ao roteador ou se ele está conectado a um comutador que está conectado ao roteador.  
  
-   Verifique se o dispositivo de banda larga que se conecta ao seu provedor de serviços de Internet (ISP) está ativado, funcione corretamente, e o roteador está conectado ao dispositivo de banda larga.  
  
-   Ative a configuração UPnP do roteador. Conectar-se à página da web de configuração do roteador para ativar a configuração UPnP. Para obter informações sobre como fazer logon em seu roteador e como ativar a configuração UPnP, consulte a documentação do roteador. Depois de ativar a configuração UPnP, execute o ativar em remoto da Web acesso assistente novamente para configurar o roteador.  
  
-   Se o roteador não oferece suporte total o UPnP padrão, ele não pode ser configurado automaticamente. Você deve configurar seu roteador ou comprar um roteador que suporta o padrão de UPnP manualmente.  
  
     Para configurar manualmente o roteador você, conclua as seguintes tarefas:  
  
    -   Crie reserva de endereço IP do servidor Windows Server Essentials.  
  
         Antes de configurar manualmente o roteador para encaminhar as portas necessárias ao Windows Server Essentials, você deve configurar uma reserva Dynamic Host Configuration Protocol (DHCP) para o servidor que está executando o Windows Server Essentials no roteador. Esta etapa garante que o endereço IP encaminhar as portas não mudar.  
  
         Para obter informações sobre como configurar manualmente uma reserva de DHCP para o servidor no roteador, consulte a documentação de s do fabricante do roteador.  
  
    -   Configure o encaminhamento de porta no roteador para as seguintes portas:  
  
        |Serviço ou protocolo|Porta|  
        |-------------------------|----------|  
        |HTTP|TCP 80|  
        |HTTPS|TCP 443|  
  
     Para obter informações sobre como configurar manualmente o encaminhamento de porta no roteador, consulte a documentação do fabricante s.  
  
     Uma página de configuração de roteador típico inclui uma tabela que é semelhante ao seguinte.  
  
    > [!NOTE]
    >  Nesta tabela, o endereço IP do computador que esteja executando o Windows Server Essentials é 192.168.0.100. Você deve determinar o endereço IP do seu computador e substituir esse endereço IP para o endereço IP mostrado na tabela.  
  
    |Endereço IP|Protocolo TCP/UDP|Agendamento|Filtro de entrada|  
    |----------------|---------------------------|--------------|--------------------|  
    |192.168.0.100|TCP 80|Sempre|Permitir tudo|  
    |192.168.0.100|TCP 443|Sempre|Permitir tudo|  
  
     Depois de configurar manualmente o roteador, execute a ative em Web Assistente de acesso remoto, garantindo que você selecione o **configuração de roteador ignorar** opção no **Introdução** página.  
  
-   Compre um novo roteador se o roteador não oferece suporte total o UPnP padrão.  
  
> [!TIP]
>  Certifique-se de que seu roteador tem o firmware do BIOS mais recente instalado. Geralmente, você pode atualizar o firmware do BIOS para o seu roteador da página da web de configuração para o roteador. Para obter mais informações, consulte a documentação do roteador. Depois que o roteador for atualizado, execute o Assistente de configuração de qualquer lugar acesso.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Use o acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o acesso via Web remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o acesso em qualquer lugar](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  

-   [Suporte ao Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Suporte ao Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

