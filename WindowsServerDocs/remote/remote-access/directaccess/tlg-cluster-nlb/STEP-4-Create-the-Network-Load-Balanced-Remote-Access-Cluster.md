---
title: ETAPA 4 criar o cluster de acesso remoto com balanceamento de carga de rede
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess em um cluster com o NLB do Windows para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 509eaa08-c49d-448d-a71e-c1c45519ccd5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e35b9cbbe050017ba8773712e50d188a35447ee3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819229"
---
# <a name="step-4-create-the-network-load-balanced-remote-access-cluster"></a>ETAPA 4 criar o cluster de acesso remoto com balanceamento de carga de rede

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

 O Windows Server 2016, o Windows Server 2012 R2 e o Windows Server 2012 permitem criar clusters de servidores de acesso remoto. Um cluster atua como um único servidor lógico e fornece configuração e gerenciamento centralizados para os servidores no cluster. Ao usar o NLB (balanceamento de carga de rede), há suporte para até 8 membros de acesso remoto em um único cluster. Os clusters de acesso remoto fornecem alta disponibilidade e balanceamento de carga de conexões de clientes DirectAccess para a rede interna.  
  
Os procedimentos a seguir permitem criar e testar um cluster de acesso remoto:  
  
1. Instale o recurso de balanceamento de carga de rede em EDGE1 e EDGE2. Antes de habilitar o balanceamento de carga, você deve instalar o recurso de balanceamento de carga de rede em EDGE1 e EDGE2.
  
2. Habilite o balanceamento de carga em EDGE1. O EDGE1 foi originalmente instalado em modo de servidor único. Para habilitar o balanceamento de carga, configure novos endereços IP dedicados internos e externos (DIPs) para EDGE1. Os DIPs anteriores no EDGE1 são automaticamente configurados como VIPs (endereços IP virtuais) para o cluster. O novo DIP externo é 131.107.0.10, o novo DIP IPv4 interno é 10.0.0.10, o novo DIP IPv6 interno é 2001: DB8:1:: 10. Os VIPs de cluster são 131.107.0.2 e 131.107.0.3 (External) e 10.0.0.2 e 2001: DB8:1:: 2 (interno).
  
3. Adicione EDGE2 ao cluster de balanceamento de carga. Depois de habilitar o balanceamento de carga, agora você pode adicionar EDGE2 ao cluster para fornecer balanceamento de carga e alta disponibilidade para conexões de cliente do DirectAccess.

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

Se você estiver criando este laboratório de teste em máquinas virtuais, deverá habilitar a falsificação de endereço MAC em EDGE1 e EDGE2.  
  
### <a name="enable-mac-address-spoofing-on-edge1-and-edge2"></a>Habilitar falsificação de endereço MAC em EDGE1 e EDGE2  
  
1.  Execute um desligamento normal em EDGE1 e EDGE2.  
  
2.  No computador que hospeda suas máquinas virtuais, no **Gerenciador do Hyper-V**, clique com o botão direito do mouse em EDGE1 e clique em **configurações**.  
  
3.  Na caixa de diálogo **configurações** , na lista **hardware** , clique no adaptador de rede conectado à rede corpnet e, no painel de detalhes, marque a caixa de seleção **habilitar falsificação de endereços MAC** .  
  
4.  Na lista **hardware** , clique no adaptador de rede conectado à rede da Internet e, no painel de detalhes, marque a caixa de seleção **habilitar falsificação de endereços MAC** .  
  
5.  Na caixa de diálogo **configurações** , clique em **OK**.  
  
6.  Repita este procedimento da etapa 2 em EDGE2.  
  
## <a name="install-the-network-load-balancing-feature-on-edge1-and-edge2"></a>Instalar o recurso de balanceamento de carga de rede em EDGE1 e EDGE2  
Para configurar o EDGE1 e o EDGE2 em um cluster, você deve instalar o recurso de balanceamento de carga de rede no EDGE1 e no EDGE2.  
  
### <a name="to-install-network-load-balancing"></a>Para instalar o balanceamento de carga de rede  
  
1.  No EDGE1, no console do Gerenciador do Servidor, no **painel**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** quatro vezes para acessar a tela de seleção de recursos do servidor.  
  
3.  Na caixa de diálogo **selecionar recursos** , selecione **balanceamento de carga de rede**, clique em **Adicionar recursos**, clique em **Avançar**e em **instalar**.  
  
4.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem sucedida e, em seguida, clique em **Fechar**.  
  
5.  Repita esse procedimento em EDGE2.  
  
## <a name="enable-load-balancing-on-edge1"></a>Habilitar o balanceamento de carga no EDGE1  
Use este procedimento para habilitar o balanceamento de carga e configurar o novo DIPs em EDGE1.  
  
### <a name="enable-load-balancing"></a>Habilitar balanceamento de carga  
  
1.  Em EDGE1, clique em **Iniciar**, digite **RAMgmtUI. exe**e pressione Enter. Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
2.  No console de gerenciamento de acesso remoto, no painel esquerdo, clique em **configuração**e, no painel **tarefas** , clique em **habilitar balanceamento de carga**.  
  
3.  No assistente habilitar balanceamento de carga, clique em **Avançar**.  
  
4.  Na página **método de balanceamento de carga** , clique em **usar NLB (balanceamento de carga de rede) do Windows**e clique em **Avançar**.  
  
5.  Na página **endereços IP dedicados externos** , na caixa **endereço IPv4** , digite **131.107.0.10**, na caixa máscara de **sub-rede** , verifique se o prefixo de sub-rede é **255.255.255.0**e clique em **Avançar**.  
  
6.  Na página **endereços IP dedicados internos** , faça o seguinte e clique em **Avançar**:  
  
    1.  Na caixa **endereço IPv4** , digite **10.0.0.10** e, na caixa **máscara de sub-rede** , verifique se o prefixo de sub-rede é **255.255.255.0**.  
  
    2.  Na caixa **endereço IPv6** , digite **2001: DB8:1:: 10** e, no comprimento do prefixo de sub-rede, verifique se o valor é **64**.  
  
7.  Na página **Resumo** , clique em **confirmar**.  
  
8.  Na caixa de diálogo **habilitar balanceamento de carga** , clique em **fechar**.  
  
9. No assistente habilitar balanceamento de carga, clique em **fechar**.  
  
## <a name="add-edge2-to-the-load-balanced-cluster"></a>Adicionar EDGE2 ao cluster com balanceamento de carga  
Use este procedimento para adicionar EDGE2 ao cluster NLB.  
  
> [!NOTE]  
> Você deve aguardar dois minutos depois de concluir as etapas anteriores antes de continuar. Depois de habilitar o NLB, o RAConfigTask é executado e define a máquina com as configurações de NLB. Isso pode levar alguns minutos para ser concluído e, se o administrador executar outra configuração relacionada a NLB antes de a tarefa terminar, essa configuração falhará.  
  
### <a name="add-edge2-to-the-cluster"></a>Adicionar EDGE2 ao cluster  
  
1.  No computador EDGE1 ou na máquina virtual, no console de gerenciamento de acesso remoto, no painel **tarefas** , em **cluster com balanceamento de carga**, clique em **Adicionar ou remover servidores**.  
  
2.  Na caixa de diálogo **Adicionar ou remover servidores** , clique em **Adicionar servidor**.  
  
3.  No assistente **Adicionar um servidor** , na página **selecionar servidor** , digite **EDGE2**e clique em **Avançar**.  
  
4.  Na página **adaptadores de rede** , em **adaptador externo**, verifique se **Internet** está selecionado e, em **adaptador interno**, verifique se **corpnet** está selecionado. Clique em **procurar**, na caixa de diálogo **segurança do Windows** , verifique se **certificado IP-HTTPS** está selecionado, clique em **OK**e, em seguida, clique em **Avançar**.  
  
5.  Na página **Resumo** , clique em **Adicionar**.  
  
6.  Na página **Conclusão**, clique em **Fechar**.  
  
7.  Na caixa de diálogo **Adicionar ou remover servidores** , clique em **confirmar**.  
  
8.  Na caixa de diálogo **adicionando e removendo servidores** , clique em **fechar**.  
  
9. Na tela **Iniciar** , digite**Nlbmgr. exe**e pressione Enter. Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
10. No **Gerenciador de balanceamento de carga de rede**, clique em cluster do **da interno**. No painel de detalhes, certifique-se de que **EDGE1 (corpnet)** e **EDGE2 (corpnet)** tenham o status **convergido**.  
  
11. Se um servidor não for **convergido**, na árvore de console, clique com o botão direito do mouse no servidor, aponte para **controlar host**e clique em **Iniciar**.  
  
12. No **Gerenciador de balanceamento de carga de rede**, clique em **cluster da Internet**. Certifique-se de que, no painel de detalhes, **EDGE1 (Internet)** e **EDGE2 (Internet)** tenham o status **convergido**.  
  
13. Se um servidor não for **convergido**, na árvore de console, clique com o botão direito do mouse no servidor, aponte para **controlar host**e clique em **Iniciar**.
