---
title: ETAPA 10 instalar e configurar o 2-EDGE1
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d98d6f7a-a2e6-45b1-9c63-08e2986a5c03
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7d21d80f4970a501e31a053483c37268bdddb811
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314610"
---
# <a name="step-10-install-and-configure-2-edge1"></a>ETAPA 10 instalar e configurar o 2-EDGE1

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

a configuração 2-EDGE1 consiste no seguinte:  
  
- Instale o sistema operacional em 2-EDGE1. Instale o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 em 2-EDGE1.  
  
- Configurar as propriedades de TCP/IP. Configure 2-EDGE1 com endereços estáticos em ambas as interfaces de rede.  
  
- Configure o roteamento entre sub-redes. Para habilitar a comunicação entre as sub-redes corpnet e 2-corpnet, você deve configurar o roteamento.  
  
- Junte-se a 2-EDGE1 ao domínio CORP2. Junte-se a 2-EDGE1 ao domínio corp2.corp.contoso.com.  
  
- Obtenha certificados em 2-EDGE1. Os certificados são necessários para a conexão IPsec entre os clientes DirectAccess e o servidor de acesso remoto e para autenticar o ouvinte IP-HTTPS quando os clientes se conectam via HTTPS.  
  
- Fornecer acesso ao CORP\User1. O usuário CORP\User1 é o administrador de acesso remoto. Para permitir que esse usuário faça alterações em 2-EDGE1 do EDGE1, você deve conceder acesso ao usuário.  
  
- Instale a função de acesso remoto em 2-EDGE1. Para habilitar uma implantação multissite, você deve instalar a função de acesso remoto em 2-EDGE1.  
  
2-EDGE1 deve ter dois adaptadores de rede instalados.  
  
## <a name="install-the-operating-system-on-2-edge1"></a><a name="installOS"></a>Instalar o sistema operacional em 2-EDGE1  
  
1.  Inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Siga as instruções para concluir a instalação, especificando o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 (instalação completa) e uma senha forte para a conta de administrador local. Faça logon usando a conta do Administrador local.  
  
3.  Conecte-se a 2-EDGE1 a uma rede que tenha acesso à Internet e execute Windows Update para instalar as atualizações mais recentes do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e desconecte-se da Internet.  
  
4.  Conecte um adaptador de rede à sub-rede 2-corpnet e a outra à Internet simulada.  
  
## <a name="configure-tcpip-properties"></a><a name="tcpip"></a>Configurar as propriedades de TCP/IP  
  
1.  No console do Gerenciador do Servidor, clique em **servidor local**e, na área **Propriedades** , ao lado de **conexão Ethernet com fio**, clique no link.  
  
2.  Em **conexões de rede**, clique com o botão direito do mouse na conexão de rede que está conectada à sub-rede de 2 corpnet, clique em **renomear**, digite **2-corpnet**e pressione Enter.  
  
3.  Clique com o botão direito do mouse em **2-corpnet**e clique em **Propriedades**.  
  
4.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
5.  Clique em **Usar o seguinte endereço IP**. Em **endereço IP**, digite **10.2.0.20**, em **máscara de sub-rede**, digite **255.255.255.0**.  
  
6.  Clique em **Usar os seguintes endereços de servidor DNS**. Em **servidor DNS preferencial**, digite **10.2.0.1**e, em **servidor DNS alternativo**, digite **10.0.0.1**.  
  
7.  Clique em **Avançado** e clique na guia **DNS**.  
  
8.  Em **sufixo DNS para essa conexão**, digite **corp2.Corp.contoso.com**e clique em **OK** duas vezes.  
  
9. Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
10. Clique em **usar o seguinte endereço IPv6**. Em **endereço IPv6**, digite **2001: DB8:2:: 20**, no **comprimento do prefixo da sub-rede**, digite **64**. Clique em **usar os seguintes endereços de servidor DNS**e, em **servidor DNS preferencial**, digite **2001: DB8:2:: 1**, em **servidor DNS alternativo**, digite **2001: DB8:1:: 1**.  
  
11. Clique em **Avançado** e clique na guia **DNS**.  
  
12. Em **sufixo DNS para essa conexão**, digite **corp2.Corp.contoso.com**e clique em **OK** duas vezes.  
  
13. Na caixa de diálogo **Propriedades de 2-corpnet** , clique em **fechar**.  
  
14. Na janela **conexões de rede** , clique com o botão direito do mouse na conexão de rede que está conectada à sub-rede da Internet, clique em **renomear**, digite **Internet**e pressione Enter.  
  
15. Clique com o botão direito do mouse em **Internet** e, em seguida, clique em **Propriedades**.  
  
16. Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
17. Clique em **Usar o seguinte endereço IP**. Em **endereço IP**, digite **131.107.0.20**. Em **Máscara de sub-rede**, digite **255.255.255.0**.  
  
18. Clique em **Avançado**. Na guia **Configurações IP**, em na área **Endereços IP**, clique em **Adicionar**. Na caixa de diálogo **endereço TCP/IP** , em tipo de **endereço IP** **131.107.0.21**, **em máscara de sub-rede** , digite **255.255.255.0**e clique em **Adicionar**.  
  
19. Clique na guia **DNS**.  
  
20. Em **sufixo DNS para esta conexão**, digite **ISP.example.com**, clique em **OK** duas vezes e, em seguida, clique em **fechar**.  
  
21. Feche a janela **Conexões de Rede**.  
  
## <a name="configure-routing-between-subnets"></a><a name="routing"></a>Configurar o roteamento entre sub-redes  
  
1.  Na tela **Iniciar** , digite**cmd. exe**e pressione Enter.  
  
2.  Na janela do prompt de comando, digite os comandos a seguir. Depois de inserir cada comando, pressione ENTER.  
  
    ```  
    netsh interface IPv4 add route 10.0.0.0/24 2-Corpnet 10.2.0.254  
    netsh interface IPv6 add route 2001:db8:1::/64 2-Corpnet 2001:db8:2::fe  
    ```  
  
3.  Para verificar a comunicação de rede entre 2-EDGE1 e DC1, digite **ping DC1.Corp.contoso.com**.  
  
4.  Verifique se há quatro respostas do endereço IPv4, 10.0.0.1 ou do endereço IPv6, 2001: DB8:1:: 1.  
  
5.  Feche a janela do Prompt de Comando.  
  
## <a name="join-2-edge1-to-the-corp2-domain"></a><a name="Join"></a>Junção 2-EDGE1 ao domínio CORP2  
  
1.  No console do Gerenciador do Servidor, no **servidor local**, na área **Propriedades** , ao lado de **nome do computador**, clique no link.  
  
2.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
3.  Na caixa de diálogo **alterações no nome do computador/domínio** , em **nome do computador**, digite **2-EDGE1**. Em **membro de**, clique em **domínio**, digite **corp2.Corp.contoso.com**e clique em **OK**.  
  
4.  Se for solicitado um nome de usuário e uma senha, digite **administrador** e sua senha e clique em **OK**.  
  
5.  Quando você vir uma caixa de diálogo de boas-vindas ao domínio corp2.corp.contoso.com, clique em **OK**.  
  
6.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
8.  Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
9. Depois que o computador for reiniciado, clique em **Alternar usuário**e, em seguida, clique em **outro usuário** e faça logon no domínio CORP2 com a conta de administrador.  
  
## <a name="obtain-certificates-on-2-edge1"></a><a name="certs"></a>Obter certificados em 2-EDGE1  
  
1.  Na tela **Iniciar** , digite**MMC. exe**e pressione Enter.  
  
2.  No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar**, **Conta do computador**, **Avançar**, **Computador local**, **Concluir** e, por fim, em **OK**.  
  
4.  Na árvore de console do snap-in certificados, abra **certificados (computador local) \Personal**.  
  
5.  Clique com o botão direito do mouse em **pessoal**, aponte para **todas as tarefas**e clique em **solicitar novo certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Na página **solicitar certificados** , marque as caixas de seleção **autenticação de cliente** e **servidor Web** e, em seguida, clique em **mais informações são necessárias para se registrar nesse certificado**.  
  
8.  Na caixa de diálogo **Propriedades do certificado** , na guia **assunto** , na área **nome da entidade** , em **tipo**, selecione **nome comum**.  
  
9. Em **valor**, digite **2-EDGE1.contoso.com**e clique em **Adicionar**.  
  
10. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
11. Em **valor**, insira **2-EDGE1.contoso.com**e clique em **Adicionar**.  
  
12. Na guia **geral** , em **nome amigável**, digite **certificado IP-HTTPS**.  
  
13. Clique em **OK**, **Registrar** e em **Concluir**.  
  
14. No painel de detalhes do snap-in certificados, verifique se um novo certificado com o nome 2-edge1.contoso.com foi registrado com finalidades pretendidas de autenticação de servidor e se um novo certificado com o nome 2-edge1.corp2.corp.contoso.com foi registrado com Propósitos pretendidos de autenticação de cliente e autenticação de servidor.  
  
15. Feche a janela do console. Se for solicitado que você salve as configurações, clique em **não**.  
  
## <a name="provide-access-to-corpuser1"></a><a name="Access"></a>Fornecer acesso ao CORP\User1  
  
1.  Na tela **Iniciar** , digite**compmgmt. msc**e pressione Enter.  
  
2.  No painel esquerdo, clique em **usuários e grupos locais**.  
  
3.  Clique duas vezes em **grupos**e clique duas vezes em **Administradores**.  
  
4.  Na caixa de diálogo **Propriedades de administradores** , clique em **Adicionar**e, na caixa de diálogo **Selecionar usuários, computadores, contas de serviço ou grupos** , clique em **locais**.  
  
5.  Na caixa de diálogo **locais** , na árvore **local** , clique em **Corp.contoso.com**e em **OK**.  
  
6.  Em **Inserir os nomes de objeto a serem selecionados** , digite **Usuário1**e clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades de administradores** , clique em **OK**.  
  
8.  Feche a janela Gerenciamento do Computador.  
  
## <a name="install-the-remote-access-role-on-2-edge1"></a><a name="InstallDA"></a>Instalar a função de acesso remoto em 2-EDGE1  
  
1.  No console do Gerenciador do Servidor, no **painel**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.  
  
3.  Na página **Selecionar funções de servidor**, selecione **Acesso Remoto**, clique em **Adicionar Recursos** e em **Avançar**.  
  
4.  Clique em **Avançar** cinco vezes.  
  
5.  Na caixa de diálogo **Confirmar seleções de instalação**, clique em **Instalar**.  
  
6.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem sucedida e, em seguida, clique em **Fechar**.  
  


