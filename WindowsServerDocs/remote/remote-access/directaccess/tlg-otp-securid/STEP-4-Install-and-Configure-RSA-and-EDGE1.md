---
title: ETAPA 4 instalar e configurar o RSA e o EDGE1
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess com autenticação OTP e RSA SecurID para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d46ede6f-1a21-414d-b8c3-6b5c87344b9d
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1a2244422c8b625f5641fb775a2e503b096b07b5
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308612"
---
# <a name="step-4-install-and-configure-rsa-and-edge1"></a>ETAPA 4 instalar e configurar o RSA e o EDGE1

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

O RSA é o servidor RADIUS e OTP e é instalado antes de configurar o RADIUS e a OTP.  
  
Você executará as seguintes etapas para configurar a implantação RSA:  
  
1. Instale o sistema operacional no servidor RSA. Instale o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 no servidor RSA.  
  
2. Configure TCP/IP no RSA. Defina as configurações de TCP/IP no servidor RSA.  
  
3. Copie os arquivos de instalação do Gerenciador de autenticação para o servidor RSA. Depois de instalar o sistema operacional no RSA, copie os arquivos do Gerenciador de autenticação para o computador RSA.  
  
4. Ingresse o servidor RSA no domínio CORP. Junte-se à RSA ao domínio CORP.  
  
5. Desabilitar o Firewall do Windows no RSA. Desabilite o Firewall do Windows no servidor RSA.  
  
6. Instale o Gerenciador de autenticação RSA no servidor RSA. Instale o Gerenciador de autenticação RSA.  
  
7. Configure o Gerenciador de autenticação RSA. Configure o Gerenciador de autenticação.  
  
8. Criar DAProbeUser. Crie uma conta de usuário para fins de investigação.  
  
9. Instale o token de software RSA SecurID em CLIENT1. Instale o token de software RSA SecurID em CLIENT1.  
  
10. Configure o EDGE1 como um agente de autenticação RSA. Configure o agente de autenticação RSA no EDGE1.  
  
11. Configure o EDGE1 para dar suporte à autenticação OTP. Configure a OTP para o DirectAccess e verifique a configuração.  
  
## <a name="install-the-operating-system-on-the-rsa-server"></a><a name="InstallOS"></a>Instalar o sistema operacional no servidor RSA  
  
1.  No RSA, inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Siga as instruções para concluir a instalação, especificando o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 (instalação completa) e uma senha forte para a conta de administrador local. Faça logon usando a conta do Administrador local.  
  
3.  Conecte o RSA a uma rede que tenha acesso à Internet e execute Windows Update para instalar as atualizações mais recentes do Windows Server 2016, do Windows Server 2012 R2 ou do Windows Server 2012 e desconecte-se da Internet.  
  
4.  Conecte o RSA à sub-rede corpnet.  
  
## <a name="configure-tcpip-on-rsa"></a><a name="TCP"></a>Configurar TCP/IP no RSA  
  
1.  Em Tarefa de Configuração Inicial, clique em **Configurar rede**.  
  
2.  Em **conexões de rede**, clique com o botão direito do mouse em **conexão de área local**e clique em **Propriedades**.  
  
3.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
4.  Clique em **Usar o seguinte endereço IP**. Em **Endereço IP**, digite **10.0.0.5**. Em **Máscara de sub-rede**, digite **255.255.255.0**. Em **gateway padrão**, digite **10.0.0.2**. Clique em **usar os seguintes endereços de servidor DNS**, em **servidor DNS preferencial**, digite **10.0.0.1**.  
  
5.  Clique em **Avançado** e clique na guia **DNS**.  
  
6.  Em **sufixo DNS para essa conexão**, digite **Corp.contoso.com**e clique em **OK** duas vezes.  
  
7.  Na caixa de diálogo **Propriedades da conexão de área local** , clique em **fechar**.  
  
8.  Feche a janela **Conexões de Rede**.  
  
## <a name="copy-authentication-manager-installation-files-to-the-rsa-server"></a><a name="copyinstfiles"></a>Copiar arquivos de instalação do Gerenciador de autenticação para o servidor RSA  
  
1.  No servidor RSA, crie a pasta C:\RSA instalação.  
  
2.  Copie o conteúdo da mídia do RSA Authentication Manager 7,1 SP4 para a pasta de instalação do C:\RSA.  
  
3.  Crie a subpasta C:\RSA Installation\License e token.  
  
4.  Copie os arquivos de licença RSA para C:\RSA Installation\License e token.  
  
## <a name="join-the-rsa-server-to-the-corp-domain"></a><a name="JoinDomain"></a>Ingressar o servidor RSA no domínio CORP  
  
1.  Clique com o botão direito do mouse em **meu computador**e clique em **Propriedades**.  
  
2.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
3.  Em **nome do computador**, digite **RSA**. Em **membro de**, clique em **domínio**, digite **Corp.contoso.com**e clique em **OK**.  
  
4.  Quando for solicitado um nome de usuário e uma senha, digite **Usuário1** e sua senha e clique em **OK**.  
  
5.  Na caixa de diálogo domínio de boas-vindas, clique em **OK**.  
  
6.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
8.  Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
9. Depois que o computador for reiniciado, digite **Usuário1** e a senha, selecione Corp na lista suspensa **fazer logon em:** e clique em **OK**.  
  
## <a name="disable-windows-firewall-on-rsa"></a><a name="BKMK_Firewall"></a>Desabilitar o Firewall do Windows no RSA  
  
1.  Clique em **Iniciar**, em **painel de controle**, em **sistema e segurança**e em **Firewall do Windows**.  
  
2.  Clique em **Ativar ou desativar o Firewall do Windows**.  
  
3.  **Desative o Firewall do Windows** para todas as configurações.  
  
4.  Clique em **OK** e feche o Firewall do Windows.  
  
## <a name="install-rsa-authentication-manager-on-the-rsa-server"></a><a name="install"></a>Instalar o Gerenciador de autenticação RSA no servidor RSA  
  
1.  Se a mensagem de aviso de segurança aparecer a qualquer momento durante esse processo, clique em **executar** para continuar.  
  
2.  Abra a pasta de instalação do C:\RSA e clique duas vezes em **autorun. exe**.  
  
3.  Clique em **instalar agora**, clique em **Avançar**, selecione a opção superior para as Américas e clique em **Avançar**.  
  
4.  Selecione **aceito os termos do contrato de licença**e clique em **Avançar**.  
  
5.  Selecione **instância primária**e clique em **Avançar**.  
  
6.  No campo **nome do diretório:** , digite **C:\RSA**e clique em **Avançar**.  
  
7.  Verifique se o nome do servidor (RSA.corp.contoso.com) e o endereço IP estão corretos e clique em **Avançar**.  
  
8.  Navegue até C:\RSA Installation\License e token e clique em **Avançar**.  
  
9. Na página **verificar arquivo de licença** , clique em **Avançar**.  
  
10. No campo **ID de usuário** , digite **administrador**e, nos campos **senha** e **Confirmar senha** , digite uma senha forte. Clique em **Avançar**.  
  
11. Na tela de seleção de log, aceite os padrões e clique em **Avançar**.  
  
12. Na tela Resumo, clique em **instalar**.  
  
13. Após a conclusão da instalação, clique em **concluir**.  
  
## <a name="configure-rsa-authentication-manager"></a><a name="confiauthmgr"></a>Configurar o Gerenciador de autenticação RSA  
  
1.  Se o console de segurança RSA não abrir automaticamente, no computador da RSA, clique duas vezes em "RSA Security console".  
  
2.  Se o alerta de aviso/segurança do certificado de segurança for exibido, clique em **continuar neste site** ou clique em **Sim** para continuar e adicione este site a sites confiáveis, se solicitado.  
  
3.  No campo **ID de usuário** , digite **administrador** e clique em **OK**.  
  
4.  No campo **senha** , digite a senha da conta de administrador e clique em **fazer logon**.  
  
5.  Inserir informações de token.  
  
    1.  No **console de segurança RSA** , clique em **autenticação** e clique em **SecurID tokens**.  
  
    2.  Clique no **trabalho importar tokens**e, em seguida, clique em **Adicionar novo**.  
  
    3.  Na seção **Opções de importação** , clique em **procurar**. Navegue até e selecione o arquivo XML de tokens em C:\ RSA Installation\License e a pasta token e clique em **abrir**.  
  
    4.  Clique em **Enviar trabalho** na parte inferior da página.  
  
6.  Crie um novo usuário de OTP.  
  
    1.  No **console de segurança do RSA** , clique na guia **identidade** , clique em **usuários**e clique em **Adicionar novo**.  
  
    2.  Na seção **sobrenome:** digite **usuário**e, na seção ID do **usuário:** , digite **Usuário1** (UserID deve ser o mesmo que o nome de usuários do AD usado para este laboratório).  Nas seções **senha:** e **Confirmar senha:** , digite uma senha forte. Desmarque a caixa de seleção **' exigir que o usuário altere a senha no próximo logon '** e clique em **salvar**.  
  
7.  Atribua o Usuário1 a um dos tokens importados.  
  
    1.  Na página **usuários** , clique em **Usuário1** e clique em **proteger tokens**.  
  
    2.  Clique em **SecurID tokens** e clique em **atribuir token**.  
  
    3.  No cabeçalho **número de série** , clique no primeiro número listado e clique em **atribuir**.  
  
    4.  Clique no token atribuído e clique em **Editar**. Na seção **Gerenciamento de PIN SecurID** para **requisito de autenticação de usuário**, selecione **não exigir PIN (somente tokencode)** .  
  
    5.  Clique em **salvar e distribuir token**.  
  
    6.  Na página **distribuir token de software** na seção **noções básicas** , clique em **arquivo de token de emissão (SDTID)** .  
  
    7.  Na página **distribuir token de software** na seção **Opções de arquivo de token** , desmarque a caixa de seleção **habilitar proteção contra cópia** . Clique em **nenhuma senha** e em **Avançar**.  
  
    8.  Na página **distribuir token de software** na seção **baixar arquivo** , clique em **baixar agora**. Clique em **Salvar**. Navegue até C:\RSA instalação e clique em **salvar** e **fechar**.  
  
    9. Minimize o **console de segurança RSA** para uso posterior.  
  
8.  Configure o Gerenciador de autenticação como servidor RADIUS.  
  
    1.  Na área de trabalho do computador RSA, clique duas vezes em **"console de operações de segurança RSA"** .  
  
    2.  Se o alerta de aviso/segurança do certificado de segurança for exibido, clique em **continuar neste site** ou clique em **Sim** para continuar e adicione este site a sites confiáveis, se solicitado.  
  
    3.  Insira a ID de usuário e a senha e clique em **fazer logon**.  
  
    4.  Clique em **configuração de implantação-RADIUS-configurar servidor**.  
  
    5.  Na página **credenciais adicionais necessárias** , insira a ID de usuário do administrador e a senha e clique em **OK**.  
  
    6.  Na página **Configurar servidor RADIUS** , insira a mesma senha usada para o usuário administrador para os **segredos** e a **senha mestra**. Insira a ID de usuário e a senha do administrador e clique em **Configurar**.  
  
    7.  Verifique se a mensagem **' servidor RADIUS configurado com êxito '** é exibida. Clique em **Concluído**. Feche o **console de operações RSA**.  
  
    8.  Volte para o **"console de segurança RSA"** .  
  
    9. Na guia **RADIUS** , clique em **servidores RADIUS**. Verifique se rsa.corp.contoso.com está listado.  
  
9. Configure o servidor RSA como cliente de autenticação RSA.  
  
    1.  Na guia **RADIUS** , clique em **clientes RADIUS** e **adicione novo**.  
  
    2.  Clique na caixa de seleção **qualquer cliente RADIUS** .  
  
    3.  Digite uma senha forte de sua escolha no campo **segredo compartilhado** . Você usará essa mesma senha mais tarde ao configurar o EDGE1 para OTP.  
  
    4.  Deixe o campo **endereço IP** em branco e a entrada **criar/modelo** como **RADIUS padrão**.  
  
    5.  Clique em **salvar sem Agente RSA**.  
  
10. Crie arquivos necessários para configurar o EDGE1 como um agente de autenticação RSA.  
  
    1.  Na guia **acesso** , realce **agentes de autenticação**e clique em **Adicionar novo**.  
  
    2.  Digite **EDGE1** no campo **hostname** e clique em **resolver IP**.  
  
    3.  Observe que o endereço IP de EDGE1 agora é exibido no campo **endereço IP** . Clique em **Salvar**.  
  
11. Gere um arquivo de configuração para o servidor EDGE1 (AM_Config. zip).  
  
    1.  Na guia **acesso** , realce **agentes de autenticação**e clique em **gerar arquivo de configuração**.  
  
    2.  Na página **gerar arquivo de configuração** , clique em **gerar arquivo config**e clique em **baixar agora**.  
  
    3.  Clique em **salvar**, navegue até C:\ Instalação do RSA e clique em **salvar**.  
  
    4.  Clique em **fechar** na caixa de diálogo **baixar completo** .  
  
12. Gere um arquivo de segredo de nó para o servidor EDGE1 (EDGE1_NodeSecret. zip).  
  
    1.  Na guia **acesso** , realce **agentes de autenticação**e clique em **gerenciar existente**.  
  
    2.  Clique no nó EDGE1 configurado atual e clique em **gerenciar segredo do nó**.  
  
    3.  Marque a caixa de seleção **criar um novo segredo de nó aleatório e exportar o segredo do nó para um arquivo** .  
  
    4.  Insira a mesma senha usada para o usuário administrador nos campos **senha de criptografia** e **Confirmar senha de criptografia** e clique em **salvar**.  
  
    5.  Na página **gerada pelo arquivo de segredo do nó** , clique em **baixar agora**.  
  
    6.  Na caixa de diálogo **download de arquivo** , clique em **salvar**, navegue até C:\RSA instalação e clique em **salvar**. Clique em **fechar** na caixa de diálogo **baixar completo** .  
  
    7.  Na cópia de mídia do Gerenciador de autenticação RSA \ auth_mgr \Windows-x86_64 \am\rsa-ace_nsload \win32-5.0-x86\ agent_nsload. exe para a instalação do C:\RSA.  
  
## <a name="create-daprobeuser"></a><a name="BKMK_DAProbeUser"></a>Criar DAProbeUser  
  
1.  No **console de segurança do RSA** , clique na guia **identidade** , clique em **usuários**e clique em **Adicionar novo**.  
  
2.  Na seção **nome do sobrenome:** , digite **investigação**e, na seção **ID do usuário:** , digite **DAProbeUser**. Nas seções **senha:** e **Confirmar senha:** , digite uma senha forte. Desmarque a caixa de seleção **' exigir que o usuário altere a senha no próximo logon '** e clique em **salvar**.  
  
## <a name="install-rsa-securid-software-token-on-client1"></a><a name="InstToken"></a>Instalar o token de software RSA SecurID em CLIENT1  
Use este procedimento para instalar o token de software SecurID em CLIENT1.  
  
#### <a name="install-securid-software-token"></a>Instalar o token de software SecurID  
  
1.  No computador CLIENT1, crie a pasta C:\RSA files. Copie o arquivo Software_Tokens. zip da instalação do C:\RSA no computador RSA para arquivos C:\RSA. Extraia o arquivo User1_000031701832. SDTID para arquivos C:\RSA em CLIENT1.  
  
2.  Acesse a fonte de mídia do token de software RSA SecurID e clique duas vezes em RSASECURIDTOKEN410 na pasta do **aplicativo cliente SecurID SoftwareToken** para iniciar a instalação RSA SecurID. Se a mensagem **Abrir arquivo – aviso de segurança** for exibida, clique em **executar**.  
  
3.  Na caixa de diálogo **RSA SecurID software token-InstallShield Wizard,** clique em **Avançar** duas vezes.  
  
4.  Aceite o contrato de licença e clique em **Avançar**.  
  
5.  Na caixa de diálogo **tipo de instalação** , selecione **típica**, clique em **Avançar**e clique em **instalar**.  
  
6.  Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
7.  Marque a caixa de seleção **Iniciar token de software RSA SecurID** e clique em **concluir**.  
  
8.  Clique em **Importar do arquivo**.  
  
9. Clique em **procurar**, selecione C:\RSA arquivos \ USER1_000031701832. SDTID e clique em **abrir**.  
  
10. Clique em **OK** duas vezes.  
  
## <a name="configure-edge1-as-an-rsa-authentication-agent"></a><a name="configAuthAgt"></a>Configurar o EDGE1 como um agente de autenticação RSA  
Use este procedimento para configurar o EDGE1 para executar a autenticação RSA.  
  
#### <a name="configure-the-rsa-authentication-agent"></a>Configurar o agente de autenticação RSA  
  
1. No EDGE1, abra o Windows Explorer e crie os arquivos da pasta C:\RSA. Navegue até a mídia de instalação do RSA ACE.  
  
2. Copie os arquivos agent_nsload. exe, AM_Config. zip e EDGE1_NodeSecret. zip da mídia RSA para arquivos C:\RSA.  
  
3. Extraia o conteúdo de ambos os arquivos zip para os seguintes locais:  
  
   1.  C:\Windows\System32  
  
   2.  C:\Windows\SysWOW64\  
  
4. Copie agent_nsload. exe para C:\Windows\SysWOW64\\.  
  
5. Abra um prompt de comando com privilégios elevados e navegue até C:\Windows\SysWOW64.  
  
6. Digite **agent_nsload. exe-f nodesecret. rec-p <password>** em que <password> é a senha forte que você criou durante a configuração inicial do RSA. Pressione Enter.  
  
7. Copiar C:\Windows\SysWOW64\securid para C:\Windows\System32.  
  
## <a name="configure-edge1-to-support-otp-authentication"></a><a name="configOTP"></a>Configurar o EDGE1 para dar suporte à autenticação OTP  
Use este procedimento para configurar a OTP para o DirectAccess e verificar a configuração.  
  
#### <a name="configure-otp-for-directaccess"></a>Configurar a OTP para o DirectAccess  
  
1.  No EDGE1, abra Gerenciador do Servidor e clique em **acesso remoto** no painel esquerdo.  
  
2.  Clique com o botão direito do mouse em **EDGE1** no painel servidores e selecione **Gerenciamento de acesso remoto**.  
  
3.  Clique em **configuração**.  
  
4.  Na janela de **instalação do DirectAccess** , em **etapa 2 – servidor de acesso remoto**, clique em **Editar**.  
  
5.  Clique em **Avançar** três vezes e, na **seção autenticação** , selecione autenticação de **dois fatores** e **use OTP**e verifique se a opção **usar certificados de computador** está marcada. Verifique se a autoridade de certificação raiz está definida como **CN = Corp-App1-CA**. Clique em **Avançar**.  
  
6.  Na seção **servidor RADIUS de OTP** , clique duas vezes no campo **nome do servidor** em branco.  
  
7.  Na caixa de diálogo **Adicionar um servidor RADIUS** , digite **RSA** no campo **nome do servidor** . Clique em **alterar** ao lado do campo **segredo compartilhado** e digite a mesma senha que você usou ao configurar os clientes RADIUS no servidor RSA no **novo segredo** e confirme os **novos campos secretos** . Clique em **OK** duas vezes e clique em **Avançar**.  
  
    > [!NOTE]  
    > Se o servidor RADIUS estiver em um domínio diferente do servidor de acesso remoto, o campo nome do **servidor** deverá especificar o FQDN do servidor RADIUS.  
  
8.  Na seção **servidores de AC de OTP** , selecione App1.Corp.contoso.com e clique em **Adicionar**. Clique em **Avançar**.  
  
9. Na página **modelos de certificado de OTP** , clique em **procurar** para selecionar um modelo de certificado usado para o registro de certificados emitidos para autenticação OTP e, na caixa de diálogo **modelos de certificado** , selecione **DAOTPLogon**. Clique em **OK**. Clique em **procurar** para selecionar um modelo de certificado usado para registrar o certificado usado pelo servidor de acesso remoto para assinar solicitações de registro de certificado OTP e, na caixa de diálogo **modelos de certificado** , selecione **DAOTPRA**. Clique em **OK**. Clique em **Avançar**.  
  
10. Na página **instalação do servidor de acesso remoto** , clique em **concluir**e clique em **concluir** no **Assistente de especialista do DirectAccess**.  
  
11. Na caixa de diálogo **revisão de acesso remoto** , clique em **aplicar**, aguarde a atualização da política do DirectAccess e clique em **Fechar**.  
  
12. Na tela **Iniciar** , digite**PowerShell. exe**, clique com o botão direito do mouse em **PowerShell**, clique em **avançado**e clique em **Executar como administrador**. Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
13. Na janela do Windows PowerShell, digite **gpupdate/force** e pressione Enter.  
  
14. Feche e reabra o console de gerenciamento de acesso remoto e verifique se todas as configurações de OTP estão corretas.  
  


