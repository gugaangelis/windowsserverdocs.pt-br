---
title: Monitorar a carga existente no servidor de acesso remoto
description: Este tópico faz parte do guia de monitoramento de acesso remoto e contabilização no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc232a52e82f3b66164d30a134ed9e422db0964a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282680"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>Monitorar a carga existente no servidor de acesso remoto

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

**Observação:** O Windows Server 2012 reúne o DirectAccess e o RRAS (Serviço de Roteamento e Acesso Remoto) em uma única função de Acesso Remoto.  
  
O termo **carga** refere-se às estatísticas relacionadas ao número de conexões no servidor de acesso remoto. A seguir estão as etapas necessárias para acompanhar a carga no servidor de acesso remoto.  
  
Você pode usar o painel de monitoramento está disponível no console de gerenciamento no servidor de acesso remoto para exibir as estatísticas de carga para o servidor, ou você pode usar contadores de Monitor de desempenho para acompanhar as estatísticas.  
  
> [!NOTE]  
> Você deve estar conectado como um membro do grupo Admins. do domínio ou um membro do grupo Administradores em cada computador para concluir as tarefas descritas neste tópico. Se você não puder concluir uma tarefa enquanto estiver conectado com uma conta que seja membro do grupo Administradores, tente realizar a tarefa enquanto você está conectado com uma conta que seja um membro do grupo Admins. do domínio.  
  
#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>Usar o painel de monitoramento para monitorar a carga do servidor de acesso remoto  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **PAINEL** para navegar até o **Painel de Acesso Remoto** no **Console de Gerenciamento de Acesso Remoto**.  
  
3.  No painel de monitoramento, observe o **Status de cliente remoto** lado a lado dentro de **Status do servidor** lado a lado. Esse bloco lista as estatísticas, como o número total de clientes remotos que estão conectados, o número total de clientes DirectAccess que estão conectados e o número máximo de usuários que se nas últimas 24 horas.  
  
4.  Você pode clicar em **Refresh** sob **tarefas** no painel direito para recarregar o status de integridade. Para alterar o intervalo de atualização padrão, clique em **configurar o intervalo de atualização** sob **tarefas**.  
  
#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>Para usar a ferramenta Monitor de desempenho para monitorar os contadores de desempenho no servidor de acesso remoto  
  
1.  Clique em **inicie**, clique em **ferramentas administrativas**e, em seguida, clique duas vezes em **Monitor de desempenho**.  
  
2.  Sob **desempenho**, clique em **Monitor de desempenho**.  
  
3.  Clique o **Add** botão (indicado por um ícone de cruz verde) na **Monitor de desempenho** barra de ferramentas.  
  
4.  Na lista de **contadores disponíveis**, selecione todos os contadores na **RAS** e **RAmgmtsvc** categorias e depois clique em **Adicionar >>** .  
  
5.  Novamente, na lista de **contadores disponíveis**, selecione todos os contadores na **conexões IPsec** categoria e clique **Adicionar >>.**  
  
6.  Clique em **Okey** para adicionar os contadores selecionados na **Monitor de desempenho** console para o rastreamento.  
  
**Monitor de desempenho** agora mostrará graficamente as estatísticas de carga do servidor selecionado.  
  
![Windows PowerShell](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary  
```  
  


