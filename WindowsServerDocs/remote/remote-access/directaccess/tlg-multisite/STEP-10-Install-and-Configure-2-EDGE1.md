---
title: ETAPA 10 instalar e configurar EDGE1 2
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d98d6f7a-a2e6-45b1-9c63-08e2986a5c03
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9810d7294a2651d4811bc5969eaf6a118db8ed56
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283264"
---
# <a name="step-10-install-and-configure-2-edge1"></a>ETAPA 10 instalar e configurar EDGE1 2

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Configuração 2 EDGE1 consiste no seguinte:  
  
- Instale o sistema operacional em EDGE1 2. Instale o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 em EDGE1 2.  
  
- Configurar as propriedades de TCP/IP. Configure 2 EDGE1 com endereços estáticos em ambas as interfaces de rede.  
  
- Configure o roteamento entre sub-redes. Para habilitar a comunicação entre as sub-redes da rede corporativa e da rede corporativa de 2, você deve configurar o roteamento.  
  
- Ingresse no domínio de CORP2 EDGE1 2. Ingresse no domínio de corp2.corp.contoso.com EDGE1 2.  
  
- Obter certificados em EDGE1 2. Certificados são necessários para a conexão IPsec entre os clientes DirectAccess e o servidor de acesso remoto e para autenticar o ouvinte do IP-HTTPS, quando os clientes se conectam via HTTPS.  
  
- Fornece acesso ao CORP\User1. O usuário CORP\User1 é o administrador de acesso remoto. Para habilitar esse usuário fazer alterações EDGE1 2 do EDGE1, você deve conceder acesso ao usuário.  
  
- Instale a função de acesso remoto em EDGE1 2. Para habilitar uma implantação multissite, você deve instalar a função acesso remoto em EDGE1 2.  
  
2-EDGE1 deve ter dois adaptadores de rede instalados.  
  
## <a name="installOS"></a>Instalar o sistema operacional em EDGE1 2  
  
1.  Inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Siga as instruções para concluir a instalação, especificando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (instalação completa) e uma senha forte para a conta de administrador local. Faça logon usando a conta local de administrador.  
  
3.  Conectar-se EDGE1 de 2 a uma rede que tenha acesso à Internet e execute o Windows Update para instalar as atualizações mais recentes para o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e, em seguida, desconecte da Internet.  
  
4.  Conecte-se um adaptador de rede à sub-rede Corpnet 2 e o outro à Internet simulada.  
  
## <a name="tcpip"></a>Configurar propriedades de TCP/IP  
  
1.  No console do Gerenciador do servidor, clique em **servidor Local**e, em seguida, o **propriedades** área, ao lado **Conexão de Ethernet com fio**, clique no link.  
  
2.  No **conexões de rede**, a conexão de rede que está conectado à sub-rede Corpnet 2 com o botão direito, clique em **Renomear**, digite **2 Corpnet**, e pressione ENTER.  
  
3.  Clique com botão direito **2 Corpnet**e, em seguida, clique em **propriedades**.  
  
4.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
5.  Clique em **Usar o seguinte endereço IP**. Na **endereço IP**, digite **10.2.0.20**, na **máscara de sub-rede**, tipo **255.255.255.0**.  
  
6.  Clique em **Usar os seguintes endereços de servidor DNS**. Na **servidor DNS preferencial**, digite **10.2.0.1**e, na **servidor DNS alternativo**, digite **10.0.0.1**.  
  
7.  Clique em **Avançado** e clique na guia **DNS**.  
  
8.  Na **sufixo DNS para essa conexão**, digite **corp2.corp.contoso.com**e, em seguida, clique em **Okey** duas vezes.  
  
9. Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
10. Clique em **usar o seguinte endereço IPv6**. Na **endereço IPv6**, digite **2001:db8:2::20**, na **comprimento do prefixo de sub-rede**, tipo **64**. Clique em **usar os seguintes endereços de servidor DNS**e, na **servidor DNS preferencial**, digite **2001:db8:2::1**, no **servidor DNS alternativo**, tipo de **2001:db8:1::1**.  
  
11. Clique em **Avançado** e clique na guia **DNS**.  
  
12. Na **sufixo DNS para essa conexão**, digite **corp2.corp.contoso.com**e, em seguida, clique em **Okey** duas vezes.  
  
13. Sobre o **propriedades da rede corporativa de 2** caixa de diálogo, clique em **fechar**.  
  
14. No **conexões de rede** janela, a conexão de rede que está conectado à sub-rede da Internet com o botão direito, clique em **Renomear**, digite **Internet**, e, em seguida, pressione ENTER .  
  
15. Clique com o botão direito do mouse em **Internet** e, em seguida, clique em **Propriedades**.  
  
16. Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
17. Clique em **Usar o seguinte endereço IP**. Na **endereço IP**, digite **131.107.0.20**. Em **Máscara de sub-rede**, digite **255.255.255.0**.  
  
18. Clique em **Avançado**. Na guia **Configurações IP**, em na área **Endereços IP**, clique em **Adicionar**. Sobre o **endereço TCP/IP** na caixa **endereço IP** tipo **131.107.0.21**, no **máscara de sub-rede** tipo **255.255.255.0** e, em seguida, clique em **Add**.  
  
19. Clique na guia **DNS**.  
  
20. Na **sufixo DNS para essa conexão**, digite **ISP.exemplo.com**, clique em **Okey** duas vezes e, em seguida, clique em **fechar**.  
  
21. Feche a janela **Conexões de Rede** .  
  
## <a name="routing"></a>Configurar o roteamento entre sub-redes  
  
1.  Sobre o **inicie** tela, digite**cmd.exe**, e pressione ENTER.  
  
2.  Na janela do Prompt de comando, digite os comandos a seguir. Depois de inserir cada comando, pressione ENTER.  
  
    ```  
    netsh interface IPv4 add route 10.0.0.0/24 2-Corpnet 10.2.0.254  
    netsh interface IPv6 add route 2001:db8:1::/64 2-Corpnet 2001:db8:2::fe  
    ```  
  
3.  Para verificar a comunicação de rede entre 2 EDGE1 e DC1, digite **ping dc1.corp.contoso.com**.  
  
4.  Verifique se há quatro respostas do endereço IPv4, 10.0.0.1, ou do endereço IPv6, 2001:db8:1::1.  
  
5.  Feche a janela do Prompt de Comando.  
  
## <a name="Join"></a>Ingressar no domínio de CORP2 EDGE1 2  
  
1.  No console do Gerenciador do servidor, no **servidor Local**, no **Properties** área, ao lado **nome do computador**, clique no link.  
  
2.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
3.  Sobre o **alterações de nome/domínio do computador** na caixa **nome do computador**, tipo **2 EDGE1**. No **membro**, clique em **domínio**, digite **corp2.corp.contoso.com**e, em seguida, clique em **Okey**.  
  
4.  Se você for solicitado um nome de usuário e senha, digite **Administrator** e a senha e clique **Okey**.  
  
5.  Quando você vir uma caixa de diálogo boas-vindas ao domínio corp2.corp.contoso.com, clique em **Okey**.  
  
6.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
8.  Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
9. Depois que o computador for reiniciado, clique em **trocar de usuário**e, em seguida, clique em **outro usuário** e faça logon no domínio CORP2 com a conta de administrador.  
  
## <a name="certs"></a>Obter certificados em EDGE1 2  
  
1.  Sobre o **inicie** tela, digite**mmc.exe**, e pressione ENTER.  
  
2.  No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar**, **Conta do computador**, **Avançar**, **Computador local**, **Concluir** e, por fim, em **OK**.  
  
4.  Na árvore de console do snap-in de certificados, abra **certificados (computador Local) \Personal**.  
  
5.  Clique com botão direito **pessoais**, aponte para **todas as tarefas**e, em seguida, clique em **Solicitar novo certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Sobre o **solicitar certificados** página, selecione o **autenticação cliente-servidor** e o **servidor Web** caixas de seleção e, em seguida, clique em **há mais informações necessário para se registrar neste certificado**.  
  
8.  No **propriedades do certificado** caixa de diálogo o **assunto** guia, o **nome da entidade** área, na **tipo**, selecione  **Nome comum**.  
  
9. Na **valor**, digite **2 edge1.contoso.com**e, em seguida, clique em **adicionar**.  
  
10. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
11. Na **valor**, insira **2 edge1.contoso.com**e, em seguida, clique em **adicionar**.  
  
12. Sobre o **gerais** guia **nome amigável**, tipo **certificado IP-HTTPS**.  
  
13. Clique em **OK**, **Registrar** e em **Concluir**.  
  
14. No painel de detalhes do snap-in de certificados, verifique se um novo certificado com o nome 2-edge1.contoso.com foi registrado com finalidades de autenticação de servidor, e um novo certificado com o nome 2-edge1.corp2.corp.contoso.com foi registrado com Finalidades de autenticação de cliente e servidor de autenticação.  
  
15. Feche a janela de console. Se você for solicitado a salvar as configurações, clique em **não**.  
  
## <a name="Access"></a>Fornecer acesso a CORP\User1  
  
1.  Sobre o **inicie** tela, digite**compmgmt. msc**, e, em seguida, pressione ENTER.  
  
2.  No painel esquerdo, clique em **usuários e grupos locais**.  
  
3.  Clique duas vezes em **grupos**e, em seguida, clique duas vezes em **administradores**.  
  
4.  Sobre o **propriedades de administradores** caixa de diálogo, clique em **adicionar**e, na **selecionar usuários, computadores, contas de serviço ou grupos** caixa de diálogo, clique em  **Locais**.  
  
5.  Sobre o **locais** na caixa a **local** de árvore, clique em **corp.contoso.com**e, em seguida, clique em **Okey**.  
  
6.  No **digite os nomes de objeto para selecionar** tipo **User1**e, em seguida, clique em **Okey**.  
  
7.  Sobre o **propriedades de administradores** caixa de diálogo, clique em **Okey**.  
  
8.  Feche a janela de gerenciamento do computador.  
  
## <a name="InstallDA"></a>Instalar a função de acesso remoto em EDGE1 2  
  
1.  No console do Gerenciador do servidor, nos **Dashboard**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.  
  
3.  Na página **Selecionar funções de servidor**, selecione **Acesso Remoto**, clique em **Adicionar Recursos** e em **Avançar**.  
  
4.  Clique em **Avançar** cinco vezes.  
  
5.  Na caixa de diálogo **Confirmar seleções de instalação**, clique em **Instalar**.  
  
6.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem-sucedida e clique em **Fechar**.  
  


