---
title: Etapa 2 preparar servidores de cluster
description: Este tópico faz parte do guia implantar o acesso remoto em um cluster no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 74aac416a5aa69a0cd935d58e3ecb931e4b5fd02
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308332"
---
# <a name="step-2-prepare-cluster-servers"></a>Etapa 2 preparar servidores de cluster

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Antes de configurar uma implantação de cluster, você prepara servidores adicionais para adicionar ao cluster.  
  
|{1&gt;Tarefa&lt;1}|Descrição|  
|----|--------|  
|[2,1 configurar a infraestrutura de acesso remoto](#BKMK_config)|Em cada servidor que você deseja adicionar ao cluster, configure a topologia do servidor, o endereçamento IP, o roteamento e o encaminhamento. Se você configurar um cluster com balanceamento de carga de máquinas virtuais, deverá configurar as máquinas virtuais para usar a falsificação de endereço MAC.<br /><br />Além disso, ingresse cada servidor no mesmo domínio e conecte todos os servidores à mesma sub-rede.|  
|[2,2 instalar a função de acesso remoto](#BKMK_Install)|Em cada servidor adicional que você deseja adicionar ao cluster, instale a função de acesso remoto|  
|[2,3 instalar o NLB](#BKMK_NLB)|No servidor de acesso remoto implantado e em cada servidor adicional que você deseja adicionar ao cluster, instale o recurso NLB. Observe que essa etapa não é necessária ao usar um Load Balancer externo.|  
  
## <a name="21-configure-the-remote-access-infrastructure"></a><a name="BKMK_config"></a>2,1 configurar a infraestrutura de acesso remoto  
Para configurar um cluster de acesso remoto, você deve configurar a topologia do servidor, o endereçamento IP, o roteamento e o encaminhamento em todos os servidores que irão formar parte do cluster.  
  
### <a name="to-configure-the-remote-access-infrastructure"></a>Para configurar a infraestrutura de acesso remoto  
  
1.  Configure cada um dos servidores que estarão no cluster com a mesma topologia que o primeiro servidor de acesso remoto.  
  
2.  Configure cada um dos servidores que estarão no cluster com endereçamento IP, roteamento e encaminhamento apropriados com base na configuração do primeiro servidor de acesso remoto. Observe que todos os servidores no cluster devem estar conectados à mesma sub-rede.  
  
3.  Ingresse cada um dos servidores que estarão no cluster para o mesmo domínio que o primeiro servidor de acesso remoto.  
  
## <a name="22-install-the-remote-access-role"></a><a name="BKMK_Install"></a>2,2 instalar a função de acesso remoto  
Para configurar um cluster de acesso remoto, você deve instalar a função de acesso remoto em cada servidor que formará uma parte do cluster.  
  
### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>Para instalar a função de acesso remoto em servidores VPN Always On  
  
1.  No servidor DirectAccess, no console do Gerenciador do Servidor, no **painel**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.  
  
3.  Na caixa de diálogo **selecionar funções de servidor** , selecione **acesso remoto**e clique em **Avançar**.  
  
4.  Clique em **Avançar** três vezes.  
  
5.  Na caixa de diálogo **selecionar serviços de função** , selecione **DirectAccess e VPN (RAS)** e clique em **Adicionar recursos**.  
  
6.  Selecione **Roteamento**, selecione **proxy de aplicativo Web**, clique em **Adicionar recursos**e, em seguida, clique em **Avançar**.  
  
7. Clique em **Avançar** e, em seguida, clique em **Instalar**.  
  
8.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem sucedida e, em seguida, clique em **Fechar**.  
  
9.  Repita esse procedimento em todos os servidores que você deseja que sejam membros do cluster.  
  
## <a name="23-install-nlb"></a><a name="BKMK_NLB"></a>2,3 instalar o NLB  
Para configurar um cluster de acesso remoto, você deve instalar o recurso de balanceamento de carga de rede em cada servidor que formará uma parte do cluster.  
  
> [!NOTE]  
> Esta etapa não será necessária se um balanceador de carga externo for usado.  
  
#### <a name="to-install-the-nlb-role"></a>Para instalar a função NLB  
  
1.  No servidor DirectAccess, no console do Gerenciador do Servidor, no **painel**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** quatro vezes para acessar a tela de seleção de recursos do servidor.  
  
3.  Na caixa de diálogo **selecionar recursos** , selecione **balanceamento de carga de rede**, clique em **Adicionar recursos**, clique em **Avançar**e em **instalar**.  
  
4.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem sucedida e, em seguida, clique em **Fechar**.  
  
5.  Repita esse procedimento em todos os servidores que você deseja que sejam membros do cluster.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 3: configurar um cluster com balanceamento de carga](Step-3-Configure-a-Load-Balanced-Cluster.md)  
  


