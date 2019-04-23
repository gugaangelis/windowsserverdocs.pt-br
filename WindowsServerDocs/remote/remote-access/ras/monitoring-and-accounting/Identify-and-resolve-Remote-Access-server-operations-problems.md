---
title: Identificar e resolver problemas de operações do servidor de acesso remoto
description: Este tópico faz parte do guia de monitoramento de acesso remoto e contabilização no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c58ff97e286f91610a321801d177a2977349fa78
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840457"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>Identificar e resolver problemas de operações do servidor de acesso remoto

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

**Observação:** O Windows Server 2012 reúne o DirectAccess e o RRAS (Serviço de Roteamento e Acesso Remoto) em uma única função de Acesso Remoto.  
  
Você pode usar os procedimentos a seguir para identificar problemas de operações do servidor de acesso remoto, suas causas raiz e a resolução necessária para corrigir os problemas.  
  
> [!NOTE]  
> Você deve estar conectado como um membro do grupo Admins. do domínio ou um membro do grupo Administradores em cada computador para concluir as tarefas descritas neste tópico. Se você não puder concluir uma tarefa enquanto estiver conectado com uma conta que seja membro do grupo Administradores, tente realizar a tarefa enquanto você está conectado com uma conta que seja um membro do grupo Admins. do domínio.  
  
Este tópico inclui informações sobre como executar as seguintes tarefas:  
  
- Simular um problema de operações  
  
- Identificar o problema de operações e tomar uma ação corretiva  
  
- Restaurar o serviço Auxiliar de IP  
  
### <a name="BKMK_Simulate"></a>Simular um problema de operações  
  
> [!CAUTION]  
> Como acesso remoto servidor provavelmente está configurado corretamente e não com problemas, você pode usar o procedimento a seguir para simular um problema de operações. Se seu servidor no momento está atendendo a clientes em um ambiente de produção, não convém executar estas ações no momento. Em vez disso, você pode ler as etapas para compreender como resolver problemas que possam surgir no servidor de acesso remoto no futuro.  
  
As auxiliar de IP (IPHlpSvc) hosts IPv6 em transição tecnologias de serviços (como Teredo, 6to4 ou IP-HTTPS) e é necessário para o servidor DirectAccess funcionar corretamente. Para demonstrar um problema simulado operações no servidor de acesso remoto, você deve interromper o serviço de rede (IPHlpSvc).  
  
##### <a name="to-stop-the-ip-helper-service"></a>Para interromper o serviço Auxiliar de IP  
  
1.  Sobre o **iniciar** tela do servidor de acesso remoto, clique em **ferramentas administrativas**e, em seguida, clique duas vezes em **serviços**.  
  
2.  Na lista de **Services**, role para baixo e clique com botão direito **IP Helper**e, em seguida, clique em **parar**.  
  
### <a name="BKMK_Identify"></a>Identificar o problema de operações e tomar uma ação corretiva  
Desativando o serviço Auxiliar de IP causará um erro grave no servidor de acesso remoto. O painel de monitoramento mostrará o status de operações do servidor e os detalhes do problema.  
  
##### <a name="to-identify-the-details-and-take-corrective-action"></a>Para identificar os detalhes e tomar uma ação corretiva  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **PAINEL** para navegar até o **Painel de Acesso Remoto** no **Console de Gerenciamento de Acesso Remoto**.  
  
3.  Verifique se o servidor de acesso remoto está selecionado no painel esquerdo e, em seguida, no painel central, clique em **Status de operações**.  
  
4.  Você verá a lista de componentes com ícones de verdes ou vermelhas, que indicam o status operacional. Clique o **IP-HTTPS** linha na lista. Quando você tiver selecionado uma linha, os detalhes da operação são mostrados na **detalhes** painel da seguinte maneira:  
  
    **Error**  
  
    O serviço Auxiliar de IP (IPHlpSvc) foi interrompido. O DirectAccess pode não funcionar conforme o esperado. O serviço Auxiliar de IP fornece conectividade de túnel usando a plataforma de conectividade, tecnologias de transição IPv6 e IP-HTTPS.  
  
    **Faz com que**  
  
    1.  O serviço Auxiliar de IP foi interrompido.  
  
    2.  O serviço Auxiliar de IP não está respondendo.  
  
    **Resolução**  
  
    1.  Para garantir que o serviço está em execução, digite **Get-Service iphlpsc** em um prompt do Windows PowerShell.  
  
    2.  Para habilitar o serviço, digite **Start-Service iphlpsvc** em um prompt elevado do Windows PowerShell.  
  
    3.  Para reiniciar o serviço, digite **Restart-Service iphlpsvc** em um prompt elevado do Windows PowerShell.  
  
### <a name="BKMK_Restart"></a>Restaurar o serviço Auxiliar de IP  
Para restaurar o serviço Auxiliar de IP no servidor de acesso remoto, você pode seguir as etapas de resolução acima para iniciar ou reiniciar o serviço ou você pode usar o procedimento a seguir para reverter o procedimento que você usou para simular a falha do serviço Auxiliar de IP.  
  
##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>Para reiniciar o serviço Auxiliar de IP no servidor de acesso remoto  
  
1.  Sobre o **iniciar** tela, clique em **ferramentas administrativas**e, em seguida, clique duas vezes em **serviços**.  
  
2.  Na lista de **Services**, role para baixo e clique com botão direito **IP Helper**e, em seguida, clique em **iniciar**.  
  
![Windows PowerShell](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property *  
```  
  


