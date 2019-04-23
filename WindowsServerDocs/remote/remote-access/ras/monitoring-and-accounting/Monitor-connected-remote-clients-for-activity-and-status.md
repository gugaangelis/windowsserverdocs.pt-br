---
title: Monitorar a atividade e o status de clientes remotos conectados
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
ms.assetid: beb94475-b21f-46a9-ac51-bf2bb28ca94e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 468854d6e03bbc2abee3b9fce7f7960c7c25cfd3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879707"
---
# <a name="monitor-connected-remote-clients-for-activity-and-status"></a>Monitorar a atividade e o status de clientes remotos conectados

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

**Observação:** Windows Server 2012 combina o DirectAccess e o serviço de acesso remoto (RAS) em uma única função de acesso remoto.  
  
Você pode usar o console de gerenciamento no servidor de acesso remoto para monitorar o status e a atividade do cliente remoto.  
  
> [!NOTE]  
> Você deve estar conectado como um membro do grupo Admins. do domínio ou um membro do grupo Administradores em cada computador para concluir as tarefas descritas neste tópico. Se você não puder concluir uma tarefa enquanto estiver conectado com uma conta que seja membro do grupo Administradores, tente realizar a tarefa enquanto você está conectado com uma conta que seja um membro do grupo Admins. do domínio.  
  
#### <a name="to-monitor-remote-client-activity-and-status"></a>Para monitorar o status e as atividades do cliente remoto  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **REPORTING** para navegar até **relatórios de acesso remoto** no **Console de gerenciamento de acesso remoto**.  
  
3.  Clique em **Status de cliente remoto** para navegar até o cliente remoto atividade e status da interface do usuário na **Console de gerenciamento de acesso remoto**.  
  
4.  Você verá a lista de usuários que são conectados ao servidor de acesso remoto e detalhadas estatísticas sobre eles. Clique na primeira linha na lista que corresponde a um cliente. Quando você seleciona uma linha, a atividade de usuário remoto é mostrada no painel de visualização.  
  
![Windows PowerShell](../../../media/Monitor-connected-remote-clients-for-activity-and-status/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
PS> Get-RemoteAccessConnectionStatistics  
```  
  
As estatísticas do usuário podem ser filtradas, com base nas seleções de critérios, usando os campos na tabela a seguir.  
  
|Nome do campo|Valor|  
|-------|-----|  
|Nome de usuário|O nome de usuário ou o alias do usuário remoto. Caracteres curinga podem ser usados para selecionar um grupo de usuários, como contoso\\* ou \*\administrator.|  
|nome_do_host|O nome da conta do computador do usuário remoto. Um endereço IPv4 ou IPv6 também pode ser especificado.|  
|Tipo|DirectAccess ou VPN. Se DirectAccess for selecionado, todos os usuários remotos que são conectados usando DirectAccess serão listados. Se VPN for selecionado, todos os usuários remotos que são conectados usando VPN serão listados.|  
|endereço ISP|O endereço IPv4 ou IPv6 do usuário remoto.|  
|Endereço IPv4|O endereço IPv4 interno do túnel que conectam o usuário remoto à rede corporativa.|  
|Endereço IPv6|O endereço IPv6 interno do túnel que conecta o usuário remoto à rede corporativa.|  
|Protocolo/Túnel|A tecnologia de transição que é usada pelo cliente remoto. Isso é Teredo, 6to4 ou IP-HTTPS para os usuários do DirectAccess, e é PPTP, L2TP, SSTP ou IKEv2 para usuários da VPN.|  
|Recurso Acessado|Todos os usuários que estão acessando determinado recurso corporativo ou um ponto de extremidade. O valor que corresponde a esse campo é o nome do host/endereço IP do servidor.|  
|Servidor|O servidor de Acesso Remoto ao qual os clientes estão conectados. Isso é relevante apenas para implantações de cluster e multissite.|  
  
  
  


