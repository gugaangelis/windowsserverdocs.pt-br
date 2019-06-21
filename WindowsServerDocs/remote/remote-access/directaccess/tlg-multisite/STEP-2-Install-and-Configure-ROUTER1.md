---
title: Etapa 2 de instalar e configurar o roteador 1
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7d215ca234d63e7e393fbbce4d65e0803f023487
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283210"
---
# <a name="step-2-install-and-configure-router1"></a>Etapa 2 de instalar e configurar o roteador 1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste guia de laboratório de teste multissite, o computador de roteador fornece uma ponte entre as sub-redes da rede corporativa e da rede corporativa de 2 de IPv4 e IPv6 e atua como um roteador para tráfego de Teredo e IP-HTTPS.  
  
- Instalar o sistema operacional no roteador 1 
  
- Configurar propriedades de TCP/IP e renomear o computador  
  
- Desativar o firewall
  
- Configurar o roteamento e encaminhamento
  
## <a name="install-the-operating-system-on-router1"></a>Instalar o sistema operacional no roteador 1  
Primeiro, instale o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-router1"></a>Para instalar o sistema operacional no roteador 1  
  
1.  Inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (instalação completa).  
  
2.  Siga as instruções para concluir a instalação, especificando uma senha forte para a conta local de administrador. Faça logon usando a conta local de administrador.  
  
3.  Conecte o roteador 1 a uma rede que tenha acesso à Internet e execute o Windows Update para instalar as atualizações mais recentes para o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e, em seguida, desconecte da Internet.  
  
4.  Conecte o roteador 1 para as sub-redes da rede corporativa e da rede corporativa de 2.  
  
## <a name="configure-tcpip-properties-and-rename-the-computer"></a>Configurar propriedades de TCP/IP e renomear o computador  
Definir configurações de TCP/IP no roteador e renomear o computador para o roteador 1.  
  
### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>Para configurar propriedades de TCP/IP e renomear o computador  
  
1.  No console do Gerenciador do servidor, clique em **servidor Local**e, em seguida, o **propriedades** área, ao lado **Conexão de Ethernet com fio**, clique no link.  
  
2.  No **conexões de rede** janela, o adaptador de rede que está conectado à rede corporativa com o botão direito, clique em **Renomear**, digite **Corpnet**, e pressione ENTER.  
  
3.  Clique com botão direito **Corpnet**e, em seguida, clique em **propriedades**.  
  
4.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
5.  Clique em **Usar o seguinte endereço IP**. Na **endereço IP**, digite **10.0.0.254**. Na **máscara de sub-rede**, digite **255.255.255.0**e, em seguida, clique em **Okey**.  
  
6.  Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
7.  Clique em **usar o seguinte endereço IPv6**. Na **endereço IPv6**, digite **2001:db8:1::fe**. Na **comprimento do prefixo de sub-rede**, digite **64**e, em seguida, clique em **Okey**.  
  
8.  Sobre o **propriedades da rede corporativa** caixa de diálogo, clicar **fechar**.  
  
9. No **conexões de rede** janela, o adaptador de rede que está conectado à Corpnet 2 com o botão direito, clique em **Renomear**, digite **Corpnet 2**, e pressione ENTER.  
  
10. Clique com botão direito **2 Corpnet**e, em seguida, clique em **propriedades**.  
  
11. Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
12. Clique em **Usar o seguinte endereço IP**. Na **endereço IP**, digite **10.2.0.254**. Na **máscara de sub-rede**, digite **255.255.255.0**e, em seguida, clique em **Okey**.  
  
13. Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
14. Clique em **usar o seguinte endereço IPv6**. Na **endereço IPv6**, digite **2001:db8:2::fe**. Na **comprimento do prefixo de sub-rede**, digite **64**e, em seguida, clique em **Okey**.  
  
15. Sobre o **propriedades da rede corporativa de 2** caixa de diálogo, clicar **fechar**.  
  
16. Feche a janela **Conexões de Rede** .  
  
17. No console do Gerenciador do servidor, no **servidor Local**, no **Properties** área, ao lado **nome do computador**, clique no link.  
  
18. Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
19. Sobre o **alterações de nome/domínio do computador** na caixa **nome do computador**, tipo **roteador 1**e, em seguida, clique em **Okey**.  
  
20. Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
21. Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
22. Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
23. Depois que o computador for reiniciado, faça logon com a conta de administrador local.  
  
## <a name="turn-off-the-firewall"></a>Desativar o firewall  
Este computador está configurado apenas para fornecer o roteamento entre as sub-redes da rede corporativa e da rede corporativa de 2; Portanto, o firewall deve ser desativado.  
  
### <a name="to-turn-off-the-firewall"></a>Para desativar o firewall  
  
1.  Sobre o **inicie** tela, digite**WF. msc**, e pressione ENTER.  
  
2.  No Firewall do Windows com segurança avançada, nos **ações** painel, clique em **propriedades**.  
  
3.  No **Firewall do Windows com segurança avançada** caixa de diálogo a **perfil de domínio** guia **estado do Firewall**, clique em **Off**.  
  
4.  Sobre o **Firewall do Windows com segurança avançada** caixa de diálogo a **perfil privado** guia de **estado do Firewall**, clique em **Off**.  
  
5.  Sobre o **Firewall do Windows com segurança avançada** caixa de diálogo o **perfil público** guia de **estado do Firewall**, clique em **Off**e, em seguida, Clique em **Okey**.  
  
6.  Feche o Firewall do Windows com segurança avançada.  
  
## <a name="configure-routing-and-forwarding"></a>Configurar o roteamento e encaminhamento  
Para fornecer roteamento e serviços entre as sub-redes da rede corporativa e da rede corporativa de 2 de encaminhamento, você deve habilitar o encaminhamento de interfaces de rede e configurar rotas estáticas entre as sub-redes.  
  
### <a name="to-configure-static-routes"></a>Para configurar rotas estáticas  
  
1.  Sobre o **inicie** tela, digite**cmd.exe**, e pressione ENTER.  
  
2.  Habilite o encaminhamento em interfaces IPv4 e IPv6 de ambos os adaptadores de rede usando os comandos a seguir. Depois de inserir cada comando, pressione ENTER.  
  
    ```  
    netsh interface IPv4 set interface Corpnet forwarding=enabled  
    netsh interface IPv4 set interface 2-Corpnet forwarding=enabled  
    netsh interface IPv6 set interface Corpnet forwarding=enabled  
    netsh interface IPv6 set interface 2-Corpnet forwarding=enabled  
    ```  
  
3.  Habilite o roteamento de IP-HTTPS entre as sub-redes da rede corporativa e da rede corporativa de 2.  
  
    ```  
    netsh interface IPv6 add route 2001:db8:1:1000::/59 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:db8:2:2000::/59 2-Corpnet 2001:db8:2::20  
    ```  
  
4.  Habilite o Teredo roteamento entre as sub-redes da rede corporativa e da rede corporativa de 2.  
  
    ```  
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20  
    ```  
  
5.  Feche a janela do Prompt de Comando.
