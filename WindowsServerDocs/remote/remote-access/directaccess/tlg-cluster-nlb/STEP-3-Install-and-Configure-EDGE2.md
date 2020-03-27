---
title: ETAPA 3 instalar e configurar o EDGE2
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess em um cluster com o NLB do Windows para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f04eb11e-ed5f-42a1-a77b-57a248ba2d10
ms.author: lizross
author: eross-msft
ms.openlocfilehash: cb869ad1617d52562e73eb6965a9f1c2184a56a7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310782"
---
# <a name="step-3-install-and-configure-edge2"></a>ETAPA 3 instalar e configurar o EDGE2

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

EDGE2 é o segundo membro de um cluster de acesso remoto. O EDGE2 está instalado e configurado antes de habilitar a configuração do cluster.

Execute as seguintes etapas para configurar o EDGE2:

## <a name="install-the-operating-system-on-edge2"></a><a name="installOS"></a>Instalar o sistema operacional no EDGE2  
  
1.  No EDGE2, inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Siga as instruções para concluir a instalação, especificando o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 (instalação completa) e uma senha forte para a conta de administrador local. Faça logon usando a conta do Administrador local.  
  
3.  Conecte o EDGE2 a uma rede que tenha acesso à Internet e execute Windows Update para instalar as atualizações mais recentes do Windows Server 2016, do Windows Server 2012 R2 ou do Windows Server 2012 e desconecte-se da Internet.  
  
4.  Conecte um adaptador de rede à sub-rede corpnet ou ao comutador virtual que representa a sub-rede corpnet e o outro à sub-rede da Internet ou ao comutador virtual que representa a sub-rede da Internet.  
  
## <a name="configure-tcpip-properties"></a><a name="TCP"></a>Configurar as propriedades de TCP/IP  
  
1.  No console do Gerenciador do Servidor, clique em **servidor local**e, na área **Propriedades** , ao lado de **conexão Ethernet com fio**, clique no link.  
  
2.  Na janela **conexões de rede** , clique com o botão direito do mouse na conexão de rede que está conectada à sub-rede corpnet ou ao comutador virtual e clique em **renomear**.  
  
3.  Digite **corpnet**e pressione Enter.  
  
4.  Clique com o botão direito do mouse em **corpnet**e clique em **Propriedades**.  
  
5.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
6.  Clique em **Usar o seguinte endereço IP**. Em **endereço IP**, digite **10.0.0.8**. Em **Máscara de sub-rede**, digite **255.255.255.0**.  
  
7.  Clique em **Usar os seguintes endereços de servidor DNS**. Em **Servidor DNS preferencial**, digite **10.0.0.1**.  
  
8.  Clique em **Avançado** e clique na guia **DNS**.  
  
9. Em **sufixo DNS para essa conexão**, digite **Corp.contoso.com**, clique em **OK** duas vezes.  
  
10. Clique em **Protocolo IP Versão 6 (TCP/IPv6)** e em **Propriedades**.  
  
11. Clique em **usar o seguinte endereço IPv6**. Em **endereço IPv6**, digite **2001: DB8:1:: 8**. Em **comprimento do prefixo da sub-rede**, digite **64**.  
  
12. Clique em **Usar os seguintes endereços de servidor DNS**. Em **servidor DNS preferencial**, digite **2001: DB8:1:: 1**.  
  
13. Clique em **Avançado** e clique na guia **DNS**.  
  
14. Em **sufixo DNS para esta conexão**, digite **Corp.contoso.com**, clique em **OK** duas vezes e, em seguida, clique em **fechar**.  
  
15. Na janela **conexões de rede** , clique com o botão direito do mouse na conexão de rede que está conectada à sub-rede da Internet e clique em **renomear**.  
  
16. Digite **Internet**e pressione Enter.  
  
17. Clique com o botão direito do mouse em **Internet** e, em seguida, clique em **Propriedades**.  
  
18. Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
19. Clique em **Usar o seguinte endereço IP**. Em **endereço IP**, digite **131.107.0.8**. Em **máscara de sub-rede**, digite **255.255.255.0**.  
  
20. Clique na guia **DNS**  
  
21. Em **sufixo DNS para essa conexão**, digite **ISP.example.com**e clique em **OK** duas vezes e, em seguida, clique em **fechar**.  
  
22. Feche a janela **Conexões de Rede**.  
  
23. Para verificar a comunicação de rede entre EDGE2 e DC1, clique em **Iniciar**, digite **cmd**e pressione Enter.  
  
24. Na janela do prompt de comando, digite **ping DC1.Corp.contoso.com** e pressione Enter. Verifique se há quatro respostas de 10.0.0.1 ou o endereço IPv6 2001: DB8:1:: 1  
  
25. Feche a janela do Prompt de Comando.  
  
## <a name="rename-edge2-and-join-it-to-the-domain"></a><a name="rename"></a>Renomeie EDGE2 e ingresse-o no domínio  
  
1.  No console do Gerenciador do Servidor, no **servidor local**, na área **Propriedades** , ao lado de **nome do computador**, clique no link.  
  
2.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
3.  Na caixa de diálogo **alterações no nome do computador/domínio** , na caixa **nome do computador** , digite **EDGE2**. Na área **membro de** , clique em **domínio**e, na caixa de texto, digite **Corp.contoso.com**e clique em **OK**.  
  
4.  Quando seu nome de usuário e sua senha forem solicitados, digite **User1** e a senha e clique em **OK**.  
  
5.  Quando a caixa de diálogo de boas-vindas ao domínio corp.contoso.com for exibida, clique em **OK**.  
  
6.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
8.  Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
9. Após a reinicialização, faça logon como CORP\User1.  
  
## <a name="install-the-ip-https-certificate"></a><a name="IPHTTPSCert"></a>Instalar o certificado IP-HTTPS  
  
1.  Na tela **Iniciar** , digite**MMC. exe**e pressione Enter. Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
2.  No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou remover snap-ins** , clique em **certificados**, em **Adicionar**, em **conta de computador**, em **Avançar**, em **concluir**e em **OK**.  
  
4.  No painel esquerdo do console, navegue até **certificados (computador local) \Personal\Certificates**. Clique com o botão direito do mouse no nó **certificados** , aponte para **todas as tarefas**e clique em **solicitar novo certificado**.  
  
5.  No assistente de registro de certificado, clique em **Avançar** duas vezes.  
  
6.  Na página **solicitar certificados** , marque a caixa de seleção **servidor Web** e clique em **mais informações são necessárias para se registrar nesse certificado**.  
  
7.  Na caixa de diálogo **Propriedades do certificado** , na guia **assunto** , na área **nome da entidade** , na lista **tipo** , clique em **nome comum**.  
  
8.  Em **valor**, digite **EDGE1.contoso.com**e clique em **Adicionar**.  
  
9. Na área **nome alternativo** , na lista **tipo** , clique em **DNS**.  
  
10. Em **valor**, digite **EDGE1.contoso.com**e clique em **Adicionar**.  
  
11. Na guia **geral** , em **nome amigável**, digite **certificado IP-HTTPS**.  
  
12. Clique em **OK**, **Registrar** e em **Concluir**.  
  
13. No painel de detalhes do snap-in de certificados, verifique se um novo certificado com o nome edge1.contoso.com foi registrado com finalidades pretendidas de autenticação de servidor.  
  
14. Feche a janela do console. Se for solicitado que você salve as configurações, clique em **não**.  
  
## <a name="install-the-remote-access-role-on-edge2"></a><a name="InstallDA"></a>Instalar a função de acesso remoto no EDGE2  
  
1.  No console do Gerenciador do Servidor, no **painel**, clique em **adicionar funções e recursos**.  
  
2.  Clique em **Avançar** três vezes para exibir a tela de seleção de função de servidor.  
  
3.  Na página **Selecionar funções de servidor**, selecione **Acesso Remoto**, clique em **Adicionar Recursos** e em **Avançar**.  
  
4.  Clique em **Avançar** cinco vezes.  
  
5.  Na caixa de diálogo **Confirmar seleções de instalação**, clique em **Instalar**.  
  
6.  Na caixa de diálogo **Progresso da instalação**, verifique se a instalação foi bem sucedida e, em seguida, clique em **Fechar**.  
  


