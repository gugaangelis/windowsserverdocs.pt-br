---
title: Etapa 2 preparar servidores de Cluster
description: Este tópico faz parte do guia de implantação de acesso remoto em um Cluster no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 10a40b30acbf022ed34f454d753884cb8c5c97d4
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281203"
---
# <a name="step-2-prepare-cluster-servers"></a>Etapa 2 preparar servidores de Cluster

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Antes de configurar uma implantação de cluster, você pode preparar servidores adicionais a serem adicionados ao cluster.  
  
|Tarefa|Descrição|  
|----|--------|  
|[2.1 configurar a infraestrutura de acesso remoto](#BKMK_config)|Em cada servidor que você deseja adicionar ao cluster, configure a topologia do servidor, o endereçamento IP, roteamento e encaminhamento. Se você configurar um cluster com balanceamento de carga de máquinas virtuais, você deve configurar as máquinas virtuais para usar a falsificação de endereço MAC.<br /><br />Além disso, Junte-se cada servidor no mesmo domínio e se conectar a todos os servidores na mesma sub-rede.|  
|[2.2 instalar a função de acesso remoto](#BKMK_Install)|Em cada servidor adicional que você deseja adicionar ao cluster, instale a função acesso remoto|  
|[2.3 instalar o NLB](#BKMK_NLB)|No servidor de acesso remoto implantado e em cada servidor adicional que você deseja adicionar ao cluster, instale o recurso NLB. Observe que essa etapa não é necessário ao usar um balanceador de carga externo.|  
  
## <a name="BKMK_config"></a>2.1 configurar a infraestrutura de acesso remoto  
Para configurar um cluster de acesso remoto, você deve configurar a topologia do servidor, o endereçamento IP, roteamento e encaminhamento em todos os servidores que farão parte do cluster.  
  
### <a name="to-configure-the-remote-access-infrastructure"></a>Configurar a infraestrutura de acesso remoto  
  
1.  Configure cada um dos servidores que estarão no cluster com a mesma topologia como o primeiro servidor de acesso remoto.  
  
2.  Configure cada um dos servidores que estarão no cluster com apropriado IP addressing, roteamento e encaminhamento com base na configuração do primeiro servidor de acesso remoto. Observe que todos os servidores no cluster devem estar conectados à mesma sub-rede.  
  
3.  Junte-se a cada um dos servidores que estarão no cluster ao mesmo domínio como o primeiro servidor de acesso remoto.  
  
## <a name="BKMK_Install"></a>2.2 instalar a função de acesso remoto  
Para configurar um cluster de acesso remoto, você deve instalar a função acesso remoto em todos os servidores que formarão uma parte do cluster.  
  
### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>Para instalar a função de acesso remoto em servidores VPN Always On  
  
1.  No servidor do DirectAccess, no console do Gerenciador do servidor, nos **Dashboard**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.  
  
3.  Sobre o **selecionar funções do servidor** caixa de diálogo, selecione **acesso remoto**e, em seguida, clique em **próxima**.  
  
4.  Clique em **próxima** três vezes.  
  
5.  Sobre o **selecionar serviços de função** caixa de diálogo, selecione **DirectAccess e VPN (RAS)** e, em seguida, clique em **adicionar recursos**.  
  
6.  Selecione **roteamento**, selecione **Proxy de aplicativo Web**, clique em **adicionar recursos**e, em seguida, clique em **Avançar**.  
  
7. Clique em **Avançar**e, em seguida, clique em **Instalar**.  
  
8.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem-sucedida e clique em **Fechar**.  
  
9.  Repita esse procedimento em todos os servidores que você deseja ser membros do cluster.  
  
## <a name="BKMK_NLB"></a>2.3 instalar o NLB  
Para configurar um cluster de acesso remoto, você deve instalar o recurso de balanceamento de carga de rede em todos os servidores que formarão uma parte do cluster.  
  
> [!NOTE]  
> Essa etapa não será necessária se um balanceador de carga externo é usado.  
  
#### <a name="to-install-the-nlb-role"></a>Para instalar a função NLB  
  
1.  No servidor do DirectAccess, no console do Gerenciador do servidor, nos **Dashboard**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **próxima** quatro vezes para chegar à tela de seleção de recurso de servidor.  
  
3.  Sobre o **selecionar recursos** caixa de diálogo, selecione **balanceamento de carga de rede**, clique em **adicionar recursos**, clique em **Avançar**e, em seguida, clique em **Instalar**.  
  
4.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem-sucedida e clique em **Fechar**.  
  
5.  Repita esse procedimento em todos os servidores que você deseja ser membros do cluster.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 3: Configurar um cluster de balanceamento de carga](Step-3-Configure-a-Load-Balanced-Cluster.md)  
  


