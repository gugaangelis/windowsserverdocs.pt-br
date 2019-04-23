---
title: Etapa 3 Instale e Configure EDGE2
description: Este tópico faz parte do guia de laboratório de teste - demonstração do DirectAccess em um Cluster com Windows NLB para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f04eb11e-ed5f-42a1-a77b-57a248ba2d10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0692d47d50d84a66b5c3cc41d2ba2fca1004cafe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870877"
---
# <a name="step-3-install-and-configure-edge2"></a>Etapa 3 Instale e Configure EDGE2

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

EDGE2 é o segundo membro de um cluster de acesso remoto. EDGE2 está instalado e configurado antes de habilitar a configuração do cluster.

Execute as seguintes etapas para configurar EDGE2:

## <a name="installOS"></a>Instalar o sistema operacional em EDGE2  
  
1.  No EDGE2, inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Siga as instruções para concluir a instalação, especificando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (instalação completa) e uma senha forte para a conta de administrador local. Faça logon usando a conta local de administrador.  
  
3.  Conectar-se EDGE2 a uma rede que tenha acesso à Internet e execute o Windows Update para instalar as atualizações mais recentes para o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e, em seguida, desconecte da Internet.  
  
4.  Conecte-se um adaptador de rede para a sub-rede da rede corporativa ou o comutador virtual que representa a sub-rede corpnet e o outro para a sub-rede da Internet ou de um comutador virtual que representa a sub-rede da Internet.  
  
## <a name="TCP"></a>Configurar propriedades de TCP/IP  
  
1.  No console do Gerenciador do servidor, clique em **servidor Local**e, em seguida, o **propriedades** área, ao lado **Conexão de Ethernet com fio**, clique no link.  
  
2.  No **conexões de rede** janela, a conexão de rede que está conectado ao comutador virtual ou sub-rede da rede corporativa com o botão direito e, em seguida, clique em **Renomear**.  
  
3.  Tipo de **Corpnet**, e pressione ENTER.  
  
4.  Clique com botão direito **Corpnet**e, em seguida, clique em **propriedades**.  
  
5.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
6.  Clique em **Usar o seguinte endereço IP**. Na **endereço IP**, digite **10.0.0.8**. Em **Máscara de sub-rede**, digite **255.255.255.0**.  
  
7.  Clique em **Usar os seguintes endereços de servidor DNS**. Em **Servidor DNS preferencial**, digite **10.0.0.1**.  
  
8.  Clique em **Avançado** e clique na guia **DNS**.  
  
9. Na **sufixo DNS para essa conexão**, digite **corp.contoso.com**, clique em **Okey** duas vezes.  
  
10. Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
11. Clique em **usar o seguinte endereço IPv6**. Na **endereço IPv6**, digite **2001:db8:1::8**. Na **comprimento do prefixo de sub-rede**, digite **64**.  
  
12. Clique em **Usar os seguintes endereços de servidor DNS**. Na **servidor DNS preferencial**, digite **2001:db8:1::1**.  
  
13. Clique em **Avançado** e clique na guia **DNS**.  
  
14. Na **sufixo DNS para essa conexão**, digite **corp.contoso.com**, clique em **Okey** duas vezes e, em seguida, clique em **fechar**.  
  
15. No **conexões de rede** janela, a conexão de rede que está conectado à sub-rede da Internet com o botão direito e, em seguida, clique em **Renomear**.  
  
16. Tipo de **Internet**, e pressione ENTER.  
  
17. Clique com o botão direito do mouse em **Internet** e, em seguida, clique em **Propriedades**.  
  
18. Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
19. Clique em **Usar o seguinte endereço IP**. Na **endereço IP**, insira **131.107.0.8**. Na **máscara de sub-rede**, insira **255.255.255.0**.  
  
20. Clique o **DNS** guia  
  
21. Na **sufixo DNS para essa conexão**, digite **ISP.exemplo.com**e, em seguida, clique em **Okey** duas vezes e, em seguida, clique em **fechar**.  
  
22. Feche a janela **Conexões de Rede** .  
  
23. Para verificar a comunicação de rede entre EDGE2 e DC1, clique em **inicie**, digite **cmd**, e pressione ENTER.  
  
24. Na janela do Prompt de comando, digite **ping dc1.corp.contoso.com** e pressione ENTER. Verifique se há quatro respostas de 10.0.0.1 ou 2001:db8:1::1 de endereço IPv6  
  
25. Feche a janela do Prompt de Comando.  
  
## <a name="rename"></a>Renomear EDGE2 e associá-lo ao domínio  
  
1.  No console do Gerenciador do servidor, no **servidor Local**, no **Properties** área, ao lado **nome do computador**, clique no link.  
  
2.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
3.  Sobre o **alterações de nome/domínio do computador** na caixa de **nome do computador** , digite **EDGE2**. No **membro** área, clique em **domínio**e na caixa de texto, insira **corp.contoso.com**e, em seguida, clique em **Okey**.  
  
4.  Quando seu nome de usuário e sua senha forem solicitados, digite **User1** e a senha e clique em **OK**.  
  
5.  Quando você vir uma caixa de diálogo de boas-vindas ao domínio corp.contoso.com, clique em **OK**.  
  
6.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
8.  Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
9. Depois de reiniciar, faça logon como CORP\User1.  
  
## <a name="IPHTTPSCert"></a>Instalar o certificado IP-HTTPS  
  
1.  Sobre o **inicie** tela, digite**mmc.exe**, e pressione ENTER. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
3.  Sobre o **adicionar ou Remover Snap-ins** caixa de diálogo, clique em **certificados**, clique em **adicionar**, clique em **conta de computador**, clique em  **Próxima**, clique em **término**e, em seguida, clique em **Okey**.  
  
4.  No painel esquerdo do console, navegue até **certificados (computador Local) \Personal\Certificates**. Clique com botão direito do **certificados** nó, aponte para **todas as tarefas**e, em seguida, clique em **Solicitar novo certificado**.  
  
5.  No Assistente de registro de certificado, clique em **próxima** duas vezes.  
  
6.  Sobre o **solicitar certificados** página, selecione o **servidor Web** caixa de seleção e, em seguida, clique em **obter mais informações são necessárias para se registrar neste certificado**.  
  
7.  No **propriedades do certificado** caixa de diálogo a **assunto** guia o **nome da entidade** área, no **tipo** , clique em **Nome comum**.  
  
8.  Na **valor**, digite **edge1.contoso.com**e, em seguida, clique em **adicionar**.  
  
9. No **nome alternativo** área, no **tipo** , clique em **DNS**.  
  
10. Na **valor**, digite **edge1.contoso.com**e, em seguida, clique em **adicionar**.  
  
11. Sobre o **gerais** guia **nome amigável**, tipo **certificado IP-HTTPS**.  
  
12. Clique em **OK**, **Registrar** e em **Concluir**.  
  
13. No painel de detalhes do snap-in de certificados, verifique se que um novo certificado com o nome edge1.contoso.com foi registrado com finalidades de autenticação de servidor.  
  
14. Feche a janela de console. Se você for solicitado a salvar as configurações, clique em **não**.  
  
## <a name="InstallDA"></a>Instalar a função de acesso remoto em EDGE2  
  
1.  No console do Gerenciador do servidor, nos **Dashboard**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.  
  
3.  Na página **Selecionar funções de servidor**, selecione **Acesso Remoto**, clique em **Adicionar Recursos** e em **Avançar**.  
  
4.  Clique em **Avançar** cinco vezes.  
  
5.  Na caixa de diálogo **Confirmar seleções de instalação**, clique em **Instalar**.  
  
6.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem-sucedida e clique em **Fechar**.  
  


