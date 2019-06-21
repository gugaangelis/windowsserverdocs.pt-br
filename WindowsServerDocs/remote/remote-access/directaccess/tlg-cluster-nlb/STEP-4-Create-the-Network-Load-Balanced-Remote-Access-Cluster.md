---
title: Etapa 4 criar o Cluster de acesso remoto com balanceamento de carga de rede
description: Este tópico faz parte do guia de laboratório de teste - demonstração do DirectAccess em um Cluster com Windows NLB para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 509eaa08-c49d-448d-a71e-c1c45519ccd5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b8e6defc6cc8579f18df2f9636383c65c9a18eb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281618"
---
# <a name="step-4-create-the-network-load-balanced-remote-access-cluster"></a>Etapa 4 criar o Cluster de acesso remoto com balanceamento de carga de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

 Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012 permitem que você crie clusters de servidores de acesso remoto. Um cluster atua como um único servidor lógico e fornece configuração centralizada e gerenciamento para os servidores no cluster. Ao usar o balanceamento de carga de rede (NLB), há suporte para até 8 membros de acesso remoto em um único cluster. Clusters de acesso remotos fornecem alta disponibilidade e balanceamento de carga de conexões de clientes do DirectAccess à rede interna.  
  
Os procedimentos a seguir permitem que você criar e testar um cluster de acesso remoto:  
  
1. Instale o recurso de balanceamento de carga de rede em EDGE1 e EDGE2. Antes de habilitar o balanceamento de carga, você deve instalar o recurso de balanceamento de carga de rede em EDGE1 e EDGE2.
  
2. Habilite o balanceamento de carga em EDGE1. EDGE1 foi originalmente instalado no modo de servidor único. Para habilitar o balanceamento de carga, você pode configurar novos internos e externos endereços IP dedicados (DIPs) para EDGE1. Os DIPs anteriores em EDGE1 são automaticamente configurados como endereços IP virtuais (VIPs) para o cluster. O novo DIP externo é 131.107.0.10, o novo DIP interno de IPv4 é 10.0.0.10, o novo DIP IPv6 interno é 2001:db8:1::10. Os VIPs de cluster são 131.107.0.2 e 131.107.0.3 (externo) e 10.0.0.2 e 2001:db8:1::2 (interno).
  
3. Adicione EDGE2 para o cluster com balanceamento de carga. Depois de habilitar o balanceamento de carga, agora você pode adicionar EDGE2 ao cluster para fornecer a carga de balanceamento e alta disponibilidade para conexões de cliente do DirectAccess.

## <a name="prerequisites"></a>Pré-requisitos

Se você estiver criando esse laboratório de teste em máquinas virtuais, você deve habilitar a falsificação em EDGE1 e EDGE2 de endereço MAC.  
  
### <a name="enable-mac-address-spoofing-on-edge1-and-edge2"></a>Habilitar a falsificação em EDGE1 e EDGE2 de endereço MAC  
  
1.  Execute um desligamento normal em EDGE1 e EDGE2.  
  
2.  No computador que hospeda as máquinas virtuais, em **Gerenciador do Hyper-V**, clique com botão direito EDGE1 e, em seguida, clique em **configurações**.  
  
3.  No **configurações** na caixa de **Hardware** lista, clique no adaptador de rede conectado à rede da rede corporativa e, em seguida, no painel de detalhes, selecione o **permitir falsificação de endereços MAC**  caixa de seleção.  
  
4.  No **Hardware** lista, clique no adaptador de rede conectado à rede da Internet e, em seguida, no painel de detalhes, selecione a **habilitar falsificação de endereços MAC** caixa de seleção.  
  
5.  Sobre o **as configurações** caixa de diálogo, clique em **Okey**.  
  
6.  Repita esse procedimento da etapa 2 em EDGE2.  
  
## <a name="install-the-network-load-balancing-feature-on-edge1-and-edge2"></a>Instalar o recurso de balanceamento de carga de rede em EDGE1 e EDGE2  
Para configurar EDGE1 e EDGE2 em um cluster, você deve instalar o recurso de balanceamento de carga de rede em EDGE1 e EDGE2.  
  
### <a name="to-install-network-load-balancing"></a>Para instalar o balanceamento de carga de rede  
  
1.  Em EDGE1, no console do Gerenciador do servidor, nos **Dashboard**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **próxima** quatro vezes para chegar à tela de seleção de recurso de servidor.  
  
3.  Sobre o **selecionar recursos** caixa de diálogo, selecione **balanceamento de carga de rede**, clique em **adicionar recursos**, clique em **Avançar**e, em seguida, clique em **Instalar**.  
  
4.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem-sucedida e clique em **Fechar**.  
  
5.  Repita esse procedimento em EDGE2.  
  
## <a name="enable-load-balancing-on-edge1"></a>Habilitar o balanceamento de carga em EDGE1  
Use este procedimento para habilitar o balanceamento de carga e configurar os novo DIPs em EDGE1.  
  
### <a name="enable-load-balancing"></a>Habilitar o balanceamento de carga  
  
1.  Em EDGE1, clique em **inicie**, digite **RAMgmtUI.exe**, e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No console de gerenciamento de acesso remoto, no painel esquerdo, clique em **Configuration**e, em seguida, no **tarefas** painel, clique em **habilitar balanceamento de carga**.  
  
3.  No habilitar à Assistente de balanceamento de carga, clique em **próxima**.  
  
4.  Sobre o **o método de balanceamento de carga** , clique em **Use Windows Network NLB Balanceamento de carga ()** e, em seguida, clique em **próxima**.  
  
5.  No **endereços IP externos de dedicado** página, o **endereço IPv4** , digite **131.107.0.10**, no **máscara de sub-rede** , verifique se o é o prefixo de sub-rede **255.255.255.0**e, em seguida, clique em **próxima**.  
  
6.  Sobre o **endereços IP internos de dedicado** página, faça o seguinte e, em seguida, clique em **próxima**:  
  
    1.  No **endereço IPv4** , digite **10.0.0.10** e, nas **máscara de sub-rede** , verifique se o prefixo de sub-rede é **255.255.255.0**.  
  
    2.  No **endereço IPv6** , digite **2001:db8:1::10** e no comprimento do prefixo de sub-rede, verifique se o valor é **64**.  
  
7.  Sobre o **resumo** , clique em **confirmar**.  
  
8.  Sobre o **habilitar balanceamento de carga** caixa de diálogo, clique em **fechar**.  
  
9. No habilitar à Assistente de balanceamento de carga, clique em **fechar**.  
  
## <a name="add-edge2-to-the-load-balanced-cluster"></a>Adicionar EDGE2 para o cluster com balanceamento de carga  
Use este procedimento para adicionar EDGE2 ao cluster NLB.  
  
> [!NOTE]  
> Você deve aguardar dois minutos depois de concluir as etapas anteriores antes de continuar. Depois de habilitar o NLB, o RAConfigTask é executado e configura o computador com configurações de NLB. Isso pode levar alguns minutos para ser concluído e se o administrador executa outra configuração de NLB relacionados antes da conclusão da tarefa, que a configuração falhará.  
  
### <a name="add-edge2-to-the-cluster"></a>Adicionar EDGE2 ao cluster  
  
1.  No computador de EDGE1 ou máquina virtual, no Console de gerenciamento de acesso remoto, nos **tarefas** painel, em **Cluster de balanceamento de carga**, clique em **adicionar ou remover servidores**.  
  
2.  Sobre o **adicionar ou remover servidores** caixa de diálogo, clique em **Adicionar servidor**.  
  
3.  No **adicionar um servidor** assistente no **Selecionar servidor** página, digite **EDGE2**e, em seguida, clique em **Avançar**.  
  
4.  No **adaptadores de rede** página, na **adaptador externo**, certifique-se de que **Internet** estiver selecionada e, na **adaptador interno**, certifique-se que **Corpnet** está selecionado. Clique em **navegue**, no **segurança do Windows** caixa de diálogo caixa, certifique-se de que **certificado IP-HTTPS** é selecionado, clique em **Okey**e, em seguida, clique em **Próxima**.  
  
5.  Sobre o **resumo** , clique em **Add**.  
  
6.  Na página **Conclusão**, clique em **Fechar**.  
  
7.  Sobre o **adicionar ou remover servidores** caixa de diálogo, clique em **confirmar**.  
  
8.  Sobre o **adicionando e removendo servidores** caixa de diálogo, clique em **fechar**.  
  
9. Sobre o **inicie** tela, digite**nlbmgr.exe**e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
10. No **Gerenciador de balanceamento de carga de rede**, clique em **cluster interno DA**. No painel de detalhes, verifique se ambos **EDGE1(Corpnet)** e **EDGE2(Corpnet)** têm o status **convergido**.  
  
11. Se um servidor não estiver **convergido**, na árvore de console, clique com botão direito do servidor, aponte para **controle de Host**e, em seguida, clique em **iniciar**.  
  
12. No **Gerenciador de balanceamento de carga de rede**, clique em **cluster DA Internet**. Certifique-se de que no painel de detalhes, ambos **EDGE1(Internet)** e **EDGE2(Internet)** têm o status **convergido**.  
  
13. Se um servidor não estiver **convergido**, na árvore de console, clique com botão direito do servidor, aponte para **controle de Host**e, em seguida, clique em **iniciar**.
