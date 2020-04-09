---
title: Monitorar a atividade e o status de clientes remotos conectados
description: Este tópico faz parte do guia de monitoramento e contabilidade de acesso remoto no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: beb94475-b21f-46a9-ac51-bf2bb28ca94e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 949a8b4da7c85c82c247c638df09768c5d0790e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860539"
---
# <a name="monitor-connected-remote-clients-for-activity-and-status"></a>Monitorar a atividade e o status de clientes remotos conectados

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

**Observação:** O Windows Server 2012 combina o DirectAccess e o serviço de acesso remoto (RAS) em uma única função de acesso remoto.  
  
Você pode usar o console de gerenciamento do no servidor de acesso remoto para monitorar a atividade e o status do cliente remoto.  
  
> [!NOTE]  
> Você deve estar conectado como um membro do grupo Admins. do domínio ou um membro do grupo Administradores em cada computador para concluir as tarefas descritas neste tópico. Se você não puder concluir uma tarefa enquanto estiver conectado com uma conta que seja membro do grupo Administradores, tente executar a tarefa enquanto estiver conectado com uma conta que seja membro do grupo Admins. do domínio.  
  
#### <a name="to-monitor-remote-client-activity-and-status"></a>Para monitorar a atividade e o status do cliente remoto  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **relatório** para navegar até **relatórios de acesso remoto** no console de gerenciamento de **acesso remoto**.  
  
3.  Clique em **status do cliente remoto** para navegar até a atividade do cliente remoto e a interface do usuário de status no **console de gerenciamento de acesso remoto**.  
  
4.  Você verá a lista de usuários que estão conectados ao servidor de acesso remoto e estatísticas detalhadas sobre eles. Clique na primeira linha da lista que corresponde a um cliente. Quando você seleciona uma linha, a atividade de usuário remoto é mostrada no painel de visualização.  
  
![](../../../media/Monitor-connected-remote-clients-for-activity-and-status/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
```  
PS> Get-RemoteAccessConnectionStatistics  
```  
  
As estatísticas de usuário podem ser filtradas, com base nas seleções de critérios, usando os campos na tabela a seguir.  
  
|Nome do campo|{1&gt;Valor&lt;1}|  
|-------|-----|  
|Nome de Usuário|O nome de usuário ou o alias do usuário remoto. Caracteres curinga podem ser usados para selecionar um grupo de usuários, como contoso\\* ou \*\Administrator.|  
|Nome do host|O nome da conta do computador do usuário remoto. Um endereço IPv4 ou IPv6 também pode ser especificado.|  
|Tipo|DirectAccess ou VPN. Se o DirectAccess estiver selecionado, todos os usuários remotos conectados usando o DirectAccess serão listados. Se a VPN estiver selecionada, todos os usuários remotos conectados usando VPN serão listados.|  
|endereço ISP|O endereço IPv4 ou IPv6 do usuário remoto.|  
|Endereço IPv4|O endereço IPv4 interno do túnel que conecta o usuário remoto à rede corporativa.|  
|Endereço IPv6|O endereço IPv6 interno do túnel que conecta o usuário remoto à rede corporativa.|  
|Protocolo/Túnel|A tecnologia de transição usada pelo cliente remoto. Isso é Teredo, 6to4 ou IP-HTTPS para usuários do DirectAccess e é PPTP, L2TP, SSTP ou IKEv2 para usuários de VPN.|  
|Recurso Acessado|Todos os usuários que estão acessando determinado recurso corporativo ou um ponto de extremidade. O valor que corresponde a esse campo é o nome de host/endereço IP do servidor.|  
|Server|O servidor de Acesso Remoto ao qual os clientes estão conectados. Isso é relevante apenas para implantações de cluster e multissite.|  
  
  
  


