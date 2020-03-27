---
title: Monitorar a carga existente no servidor de acesso remoto
description: Este tópico faz parte do guia de monitoramento e contabilidade de acesso remoto no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0507a7f0151e1f5f8aefaf4de5a3a4f8a776971d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314190"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>Monitorar a carga existente no servidor de acesso remoto

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

**Observação:** o Windows Server 2012 reúne o DirectAccess e o RRAS (Serviço de Roteamento e Acesso Remoto) em uma única função de Acesso Remoto.  
  
O termo **carga** refere-se às estatísticas relacionadas ao número de conexões no servidor de acesso remoto. A seguir estão as etapas necessárias para acompanhar a carga no servidor de acesso remoto.  
  
Você pode usar o painel de monitoramento que está disponível no console de gerenciamento do no servidor de acesso remoto para exibir as estatísticas de carga do servidor ou pode usar contadores do monitor de desempenho para controlar as estatísticas.  
  
> [!NOTE]  
> Você deve estar conectado como um membro do grupo Admins. do domínio ou um membro do grupo Administradores em cada computador para concluir as tarefas descritas neste tópico. Se você não puder concluir uma tarefa enquanto estiver conectado com uma conta que seja membro do grupo Administradores, tente executar a tarefa enquanto estiver conectado com uma conta que seja membro do grupo Admins. do domínio.  
  
#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>Para usar o painel de monitoramento para monitorar a carga do servidor de acesso remoto  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **PAINEL** para navegar até o **Painel de Acesso Remoto** no **Console de Gerenciamento de Acesso Remoto**.  
  
3.  No painel Monitoramento, observe o bloco **status do cliente remoto** no bloco **status do servidor** . Esse bloco lista estatísticas como o número total de clientes remotos conectados, o número total de clientes do DirectAccess que estão conectados e o número máximo de usuários que se conectaram nas últimas 24 horas.  
  
4.  Você pode clicar em **Atualizar** em **tarefas** no painel direito para recarregar o status de integridade. Para alterar o intervalo de atualização padrão, clique em **Configurar intervalo de atualização** em **tarefas**.  
  
#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>Para usar a ferramenta Monitor de desempenho para monitorar contadores de desempenho no servidor de acesso remoto  
  
1.  Clique em **Iniciar**, **Ferramentas administrativas**e, em seguida, clique duas vezes em **Monitor de desempenho**.  
  
2.  Em **desempenho**, clique em **Monitor de desempenho**.  
  
3.  Clique no botão **Adicionar** (indicado por um ícone de cruz verde) na barra de ferramentas do **Monitor de desempenho** .  
  
4.  Na lista de **contadores disponíveis**, selecione todos os contadores nas categorias **RAS** e **RAmgmtsvc** e, em seguida, clique em **Adicionar > >** .  
  
5.  Novamente, na lista de **contadores disponíveis**, selecione todos os contadores na categoria **conexões IPSec** e, em seguida, clique em **Adicionar > >.**  
  
6.  Clique em **OK** para adicionar os contadores selecionados no console do **Monitor de desempenho** para rastreamento.  
  
Agora, o **Monitor de desempenho** mostrará graficamente as estatísticas de carga do servidor selecionadas.  
  
![](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary  
```  
  


