---
title: ETAPA 2 instalar e configurar o ROUTER1
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 753af61939c50e225aea09714f46cba22d3b31cb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939837"
---
# <a name="step-2-install-and-configure-router1"></a>ETAPA 2 instalar e configurar o ROUTER1

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Neste guia de laboratório de teste multissite, o computador roteador fornece uma ponte IPv4 e IPv6 entre as sub-redes corpnet e 2-corpnet e atua como um roteador para o tráfego IP-HTTPS e Teredo.

- Instalar o sistema operacional no ROUTER1

- Configurar as propriedades de TCP/IP e renomear o computador

- Desligar o firewall

- Configurar roteamento e encaminhamento

## <a name="install-the-operating-system-on-router1"></a>Instalar o sistema operacional no ROUTER1
Primeiro, instale o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012.

### <a name="to-install-the-operating-system-on-router1"></a>Para instalar o sistema operacional no ROUTER1

1.  Inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (instalação completa).

2.  Siga as instruções para concluir a instalação, especificando uma senha forte para a conta local de administrador. Faça logon usando a conta local de administrador.

3.  Conecte o ROUTER1 a uma rede que tenha acesso à Internet e execute Windows Update para instalar as atualizações mais recentes do Windows Server 2016, do Windows Server 2012 R2 ou do Windows Server 2012 e desconecte-se da Internet.

4.  Conecte o ROUTER1 às sub-redes corpnet e 2-corpnet.

## <a name="configure-tcpip-properties-and-rename-the-computer"></a>Configurar as propriedades de TCP/IP e renomear o computador
Defina as configurações de TCP/IP no roteador e renomeie o computador para ROUTER1.

### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>Para configurar as propriedades de TCP/IP e renomear o computador

1.  No console do Gerenciador do Servidor, clique em **servidor local**e, na área **Propriedades** , ao lado de **conexão Ethernet com fio**, clique no link.

2.  Na janela **conexões de rede** , clique com o botão direito do mouse no adaptador de rede que está conectado ao corpnet, clique em **renomear**, digite **corpnet**e pressione Enter.

3.  Clique com o botão direito do mouse em **corpnet**e clique em **Propriedades**.

4.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.

5.  Clique em **Usar o seguinte endereço IP**. Em **endereço IP**, digite **10.0.0.254**. Em **máscara de sub-rede**, digite **255.255.255.0**e clique em **OK**.

6.  Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.

7.  Clique em **usar o seguinte endereço IPv6**. Em **endereço IPv6**, digite **2001: DB8:1:: Fe**. Em **comprimento do prefixo da sub-rede**, digite **64**e clique em **OK**.

8.  Na caixa de diálogo **Propriedades do corpnet** , clique em **fechar**.

9. Na janela **conexões de rede** , clique com o botão direito do mouse no adaptador de rede que está conectado a 2-corpnet, clique em **renomear**, digite **2-corpnet**e pressione Enter.

10. Clique com o botão direito do mouse em **2-corpnet**e clique em **Propriedades**.

11. Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.

12. Clique em **Usar o seguinte endereço IP**. Em **endereço IP**, digite **10.2.0.254**. Em **máscara de sub-rede**, digite **255.255.255.0**e clique em **OK**.

13. Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.

14. Clique em **usar o seguinte endereço IPv6**. Em **endereço IPv6**, digite **2001: DB8:2:: Fe**. Em **comprimento do prefixo da sub-rede**, digite **64**e clique em **OK**.

15. Na caixa de diálogo **Propriedades de 2-corpnet,** clique em **fechar**.

16. Feche a janela **Conexões de Rede**.

17. No console do Gerenciador do Servidor, no **servidor local**, na área **Propriedades** , ao lado de **nome do computador**, clique no link.

18. Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.

19. Na caixa de diálogo **alterações no nome do computador/domínio** , em **nome do computador**, digite **ROUTER1**e clique em **OK**.

20. Quando você for solicitado a reiniciar o computador, clique em **OK**.

21. Na caixa de diálogo **Propriedades do Sistema**, clique em **Fechar**.

22. Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.

23. Depois que o computador for reiniciado, faça logon com a conta de administrador local.

## <a name="turn-off-the-firewall"></a>Desligar o firewall
Este computador está configurado apenas para fornecer roteamento entre as sub-redes corpnet e 2-corpnet; Portanto, o firewall deve ser desativado.

### <a name="to-turn-off-the-firewall"></a>Para desativar o firewall

1.  Na tela **Iniciar** , digite**WF. msc**e pressione Enter.

2.  No firewall do Windows com segurança avançada, no painel **ações** , clique em **Propriedades**.

3.  Na caixa de diálogo **Firewall do Windows com segurança avançada** , na **guia perfil de domínio** , em estado do **Firewall**, clique em **desativado**.

4.  Na caixa de diálogo **Firewall do Windows com segurança avançada** , na **guia perfil particular** , em **estado do firewall**, clique em **desativado**.

5.  Na caixa de diálogo **Firewall do Windows com segurança avançada** , na **guia perfil público** , em **estado do firewall**, clique em **desativado**e em **OK**.

6.  Feche Firewall do Windows com Segurança Avançada.

## <a name="configure-routing-and-forwarding"></a>Configurar roteamento e encaminhamento
Para fornecer serviços de roteamento e encaminhamento entre as sub-redes corpnet e 2-corpnet, você deve habilitar o encaminhamento nas interfaces de rede e configurar rotas estáticas entre as sub-redes.

### <a name="to-configure-static-routes"></a>Para configurar rotas estáticas

1.  Na tela **Iniciar** , digite**cmd.exe**e pressione Enter.

2.  Habilite o encaminhamento nas interfaces IPv4 e IPv6 de ambos os adaptadores de rede usando os comandos a seguir. Depois de inserir cada comando, pressione ENTER.

    ```
    netsh interface IPv4 set interface Corpnet forwarding=enabled
    netsh interface IPv4 set interface 2-Corpnet forwarding=enabled
    netsh interface IPv6 set interface Corpnet forwarding=enabled
    netsh interface IPv6 set interface 2-Corpnet forwarding=enabled
    ```

3.  Habilite o roteamento IP-HTTPS entre as sub-redes corpnet e 2-corpnet.

    ```
    netsh interface IPv6 add route 2001:db8:1:1000::/59 Corpnet 2001:db8:1::2
    netsh interface IPv6 add route 2001:db8:2:2000::/59 2-Corpnet 2001:db8:2::20
    ```

4.  Habilite o roteamento Teredo entre as sub-redes corpnet e 2-corpnet.

    ```
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20
    ```

5.  Feche a janela do Prompt de Comando.
