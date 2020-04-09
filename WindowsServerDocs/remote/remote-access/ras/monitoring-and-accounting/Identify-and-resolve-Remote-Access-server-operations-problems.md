---
title: Identificar e resolver problemas de operações do servidor de acesso remoto
description: Este tópico faz parte do guia de monitoramento e contabilidade de acesso remoto no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ae48f9f59bcc297c9bcc2b65a4d094f6d0279359
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860549"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>Identificar e resolver problemas de operações do servidor de acesso remoto

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

**Observação:** o Windows Server 2012 reúne o DirectAccess e o RRAS (Serviço de Roteamento e Acesso Remoto) em uma única função de Acesso Remoto.  
  
Você pode usar os procedimentos a seguir para identificar problemas de operações do servidor de acesso remoto, suas causas raiz e a resolução necessária para corrigir os problemas.  
  
> [!NOTE]  
> Você deve estar conectado como um membro do grupo Admins. do domínio ou um membro do grupo Administradores em cada computador para concluir as tarefas descritas neste tópico. Se você não puder concluir uma tarefa enquanto estiver conectado com uma conta que seja membro do grupo Administradores, tente executar a tarefa enquanto estiver conectado com uma conta que seja membro do grupo Admins. do domínio.  
  
Este tópico inclui informações sobre como executar as seguintes tarefas:  
  
- Simular um problema de operações  
  
- Identificar o problema de operações e tomar uma ação corretiva  
  
- Restaurar o serviço auxiliar de IP  
  
### <a name="simulate-an-operations-issue"></a><a name="BKMK_Simulate"></a>Simular um problema de operações  
  
> [!CAUTION]  
> Como o servidor de acesso remoto provavelmente está configurado corretamente e não está tendo problemas, você pode usar o procedimento a seguir para simular um problema de operações. Se o servidor estiver atendendo atualmente a clientes em um ambiente de produção, talvez você não queira executar essas ações no momento. Em vez disso, você pode ler as etapas para entender como resolver problemas que podem surgir no seu servidor de acesso remoto no futuro.  
  
O serviço auxiliar de IP (IPHlpSvc) hospeda tecnologias de transição IPv6 (como IP-HTTPS, 6to4 ou Teredo) e é necessário para que o servidor DirectAccess funcione corretamente. Para demonstrar um problema de operações simuladas no servidor de acesso remoto, você deve interromper o serviço de rede (IPHlpSvc).  
  
##### <a name="to-stop-the-ip-helper-service"></a>Para interromper o serviço auxiliar de IP  
  
1.  Na tela **inicial** do servidor de acesso remoto, clique em **Ferramentas administrativas**e clique duas vezes em **Serviços**.  
  
2.  Na lista de **Serviços**, role para baixo e clique com o botão direito do mouse em **IP Helper**e clique em **parar**.  
  
### <a name="identify-the-operations-issue-and-take-corrective-action"></a><a name="BKMK_Identify"></a>Identificar o problema de operações e tomar uma ação corretiva  
A desativação do serviço auxiliar de IP causará um erro grave no servidor de acesso remoto. O painel Monitoramento mostrará o status de operações do servidor e os detalhes do problema.  
  
##### <a name="to-identify-the-details-and-take-corrective-action"></a>Para identificar os detalhes e tomar uma ação corretiva  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **PAINEL** para navegar até o **Painel de Acesso Remoto** no **Console de Gerenciamento de Acesso Remoto**.  
  
3.  Verifique se o servidor de acesso remoto está selecionado no painel esquerdo e, no painel central, clique em **status das operações**.  
  
4.  Você verá a lista de componentes com ícones verdes ou vermelhos, que indicam seu status operacional. Clique na linha **IP-HTTPS** na lista. Quando você selecionou uma linha, os detalhes da operação são mostrados no painel de **detalhes** da seguinte maneira:  
  
    **Ao**  
  
    O serviço auxiliar de IP (IPHlpSvc) foi interrompido. O DirectAccess pode não funcionar conforme o esperado. O serviço auxiliar de IP fornece conectividade de túnel usando a plataforma de conectividade, as tecnologias de transição do IPv6 e o IP-HTTPS.  
  
    **Com**  
  
    1.  O serviço auxiliar de IP foi interrompido.  
  
    2.  O serviço auxiliar de IP não está respondendo.  
  
    **Resolução**  
  
    1.  Para garantir que o serviço esteja em execução, digite **Get-Service iphlpsvc** em um prompt do Windows PowerShell.  
  
    2.  Para habilitar o serviço, digite **Start-Service iphlpsvc** em um prompt elevado do Windows PowerShell.  
  
    3.  Para reiniciar o serviço, digite **Restart-Service iphlpsvc** em um prompt elevado do Windows PowerShell.  
  
### <a name="restore-the-ip-helper-service"></a><a name="BKMK_Restart"></a>Restaurar o serviço auxiliar de IP  
Para restaurar o serviço auxiliar de IP em seu servidor de acesso remoto, você pode seguir as etapas de resolução acima para iniciar ou reiniciar o serviço ou pode usar o procedimento a seguir para reverter o procedimento que você usou para simular a falha do serviço auxiliar de IP.  
  
##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>Para reiniciar o serviço auxiliar de IP no servidor de acesso remoto  
  
1.  Na tela **Iniciar** , clique em **Ferramentas administrativas**e, em seguida, clique duas vezes em **Serviços**.  
  
2.  Na lista de **Serviços**, role para baixo e clique com o botão direito do mouse em **IP Helper**e clique em **Iniciar**.  
  
![](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
```PowerShell
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property *  
```
