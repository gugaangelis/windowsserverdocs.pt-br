---
title: Etapa 4 instalar e configurar RSA e EDGE1
description: Este tópico faz parte do guia de laboratório de teste - demonstrar o DirectAccess com autenticação OTP e SecurID de RSA para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d46ede6f-1a21-414d-b8c3-6b5c87344b9d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5280a02568305512f868f559fe35d11dc618f2b4
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283068"
---
# <a name="step-4-install-and-configure-rsa-and-edge1"></a>Etapa 4 instalar e configurar RSA e EDGE1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

RSA é o servidor RADIUS e OTP e é instalado antes de configurar a OTP e RADIUS.  
  
Você executará as seguintes etapas para configurar a implantação de RSA:  
  
1. Instale o sistema operacional no servidor do RSA. Instale o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 no servidor do RSA.  
  
2. Configure TCP/IP em RSA. Defina configurações de TCP/IP no servidor do RSA.  
  
3. Copie arquivos de instalação do Gerenciador de autenticação para o servidor do RSA. Depois de instalar o sistema operacional em RSA, copie os arquivos do Gerenciador de autenticação para o computador RSA.  
  
4. Junte-se o servidor RSA ao domínio CORP. Junte-se RSA ao domínio CORP.  
  
5. Desabilite o Firewall do Windows em RSA. Desabilite o Firewall do Windows no servidor do RSA.  
  
6. Instale o RSA Authentication Manager no servidor do RSA. Instale o Gerenciador de autenticação RSA.  
  
7. Configure o Gerenciador de autenticação RSA. Configure o Gerenciador de autenticação.  
  
8. Crie DAProbeUser. Crie uma conta de usuário para fins de investigação.  
  
9. Instale o token de software RSA SecurID no CLIENT1. Instale o token de software RSA SecurID no CLIENT1.  
  
10. Configure EDGE1 como um agente de autenticação RSA. Configure o agente de autenticação RSA em EDGE1.  
  
11. Configure EDGE1 para dar suporte à autenticação de OTP. Configurar a OTP do DirectAccess e verifique a configuração.  
  
## <a name="InstallOS"></a>Instalar o sistema operacional no servidor do RSA  
  
1.  No RSA, inicie a instalação do Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Siga as instruções para concluir a instalação, especificando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (instalação completa) e uma senha forte para a conta de administrador local. Faça logon usando a conta local de administrador.  
  
3.  Conecte o RSA para uma rede que tenha acesso à Internet e execute o Windows Update para instalar as atualizações mais recentes para o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 e, em seguida, desconecte da Internet.  
  
4.  Conecte-se RSA à sub-rede Corpnet.  
  
## <a name="TCP"></a>Configurar TCP/IP na RSA  
  
1.  Nas tarefas de configuração inicial, clique em **configurar rede**.  
  
2.  Na **conexões de rede**, clique com botão direito **Conexão de área Local**e, em seguida, clique em **propriedades**.  
  
3.  Clique em **Protocolo TCP/IP Versão 4 (TCP/IPv4)** e clique em **Propriedades**.  
  
4.  Clique em **Usar o seguinte endereço IP**. Em **Endereço IP**, digite **10.0.0.5**. Em **Máscara de sub-rede**, digite **255.255.255.0**. Na **Gateway padrão**, digite **10.0.0.2**. Clique em **usar os seguintes endereços de servidor DNS**, na **servidor DNS preferencial**, digite **10.0.0.1**.  
  
5.  Clique em **Avançado** e clique na guia **DNS**.  
  
6.  Na **sufixo DNS para essa conexão**, digite **corp.contoso.com**e, em seguida, clique em **Okey** duas vezes.  
  
7.  Sobre o **propriedades de Conexão de área Local** caixa de diálogo, clique em **fechar**.  
  
8.  Feche a janela **Conexões de Rede** .  
  
## <a name="copyinstfiles"></a>Copiar arquivos de instalação do Gerenciador de autenticação para o servidor do RSA  
  
1.  No servidor do RSA, crie a pasta de instalação C:\RSA.  
  
2.  Copie o conteúdo da mídia RSA autenticação Manager 7.1 SP4 para a pasta de instalação C:\RSA.  
  
3.  Crie a subpasta C:\RSA Installation\License e o Token.  
  
4.  Copie os arquivos de licença do RSA para C:\RSA Installation\License e o Token.  
  
## <a name="JoinDomain"></a>Junte-se o servidor RSA ao domínio CORP  
  
1.  Clique com botão direito **meu computador**e clique em **propriedades**.  
  
2.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
3.  Na **nome do computador**, digite **RSA**. Na **membro**, clique em **domínio**, digite **corp.contoso.com**e clique em **Okey**.  
  
4.  Quando você for solicitado um nome de usuário e senha, digite **User1** e a senha e clique em **Okey**.  
  
5.  No domínio de caixa de diálogo de boas-vindas, clique em **Okey**.  
  
6.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
8.  Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
9. Depois que o computador for reiniciado, digite **User1** e a senha, selecione CORP na **faça logon em:** lista suspensa e, em seguida, clique em **Okey**.  
  
## <a name="BKMK_Firewall"></a>Desabilitar o Firewall do Windows na RSA  
  
1.  Clique em **inicie**, clique em **painel de controle**, clique em **sistema e segurança**e clique em **Windows Firewall**.  
  
2.  Clique em **ativar ou desativar o Firewall do Windows**.  
  
3.  **Desative o Firewall do Windows** para todas as configurações.  
  
4.  Clique em **Okey** e feche o Firewall do Windows.  
  
## <a name="install"></a>Instale o RSA Authentication Manager no servidor do RSA  
  
1.  Se a mensagem de aviso de segurança será exibida a qualquer momento durante esse processo, clique em **executar** para continuar.  
  
2.  Abra a pasta de instalação C:\RSA e clique duas vezes em **autorun.exe**.  
  
3.  Clique em **instalar agora**, clique em **próxima**, selecione a opção do topo das Américas e clique em **próxima**.  
  
4.  Selecione **aceito os termos do contrato de licença**e clique em **próxima**.  
  
5.  Selecione **instância primária**e clique em **próxima**.  
  
6.  No **nome do diretório:** tipo de campo **C:\RSA**e clique em **próxima**.  
  
7.  Verifique se o nome do servidor (RSA.corp.contoso.com) e o endereço IP estão corretas e clique em **próxima**.  
  
8.  Navegue até C:\RSA Installation\License e o Token e, em seguida, clique em **próxima**.  
  
9. Sobre o **Verifique se o arquivo de licença** , clique em **próxima**.  
  
10. No **ID de usuário** tipo de campo **administrador**e, nas **senha** e **Confirmar senha** campos digite uma senha forte. Clique em **Avançar**.  
  
11. Na tela de seleção de log, aceite os padrões e clique em **próxima**.  
  
12. Na tela de resumo, clique em **instalar**.  
  
13. Após a conclusão da instalação, clique em **concluir**.  
  
## <a name="confiauthmgr"></a>Configurar o Gerenciador de autenticação RSA  
  
1.  Se o Console de segurança do RSA não abrir automaticamente, clique na área de trabalho do computador RSA duas vezes em "Console de segurança do RSA".  
  
2.  Se a segurança de certificado aviso / alerta de segurança for exibida, clique em **continuar neste site** ou clique em **Sim** para prosseguir e adicionar este site aos sites confiáveis, se solicitado.  
  
3.  No **ID de usuário** tipo de campo **administrador** e clique em **Okey**.  
  
4.  No **senha** a senha da conta de administrador do tipo de campo e clique em **Log On**.  
  
5.  Inserir informações de Token.  
  
    1.  No **Console de segurança do RSA** clique em **autenticação** e clique em **SecurID Tokens**.  
  
    2.  Clique em **importação Tokens de trabalho**e, em seguida, clique em **adicionar novo**.  
  
    3.  No **opções de importação** seção clique **procurar**. Procure e selecione o arquivo XML de tokens em C:\ RSA Installation\License e pasta de Token e clique em **aberto**.  
  
    4.  Clique em **enviar trabalho** na parte inferior da página.  
  
6.  Crie novo usuário OTP.  
  
    1.  No **Console de segurança do RSA** clique a **identidade** , clique em **usuários**e clique em **adicionar novo**.  
  
    2.  No **sobrenome:** tipo de seção **usuário**e, na **ID de usuário:** tipo de seção **User1** (ID de usuário deve ser o mesmo que o nome de usuário do AD usado para Este laboratório).  No **senha:** e **Confirmar senha:** seções digite uma senha forte. Desmarque a **'Exigir que o usuário altere a senha no próximo logon'** caixa de seleção e clique em **salvar**.  
  
7.  Atribua a Usuário1 a um dos tokens importados.  
  
    1.  Sobre o **os usuários** página, clique em **User1** e clique em **SecurID Tokens**.  
  
    2.  Clique em **Tokens SecurID** e clique em **atribuir Token**.  
  
    3.  Sob o **número de série** título, clique no primeiro número listado e clique em **atribuir**.  
  
    4.  Clique o token atribuído e, em seguida, clique em **editar**. No **gerenciamento de PIN SecurID** seção para o **requisito de autenticação de usuário**, selecione **não exigem o PIN (somente código de token)** .  
  
    5.  Clique em **salvar e distribuir o Token**.  
  
    6.  No **distribuir Software Token** página na **Noções básicas** seção, clique em **arquivo de Token do problema (SDTID)** .  
  
    7.  No **distribuir Software Token** página na **opções de arquivo de Token** seção, desmarque o **Habilitar proteção contra cópia** caixa de seleção. Clique em **nenhuma senha** e **próxima**.  
  
    8.  No **distribuir Software Token** página na **baixar o arquivo** seção, clique em **baixar agora**. Clique em **Salvar**. Navegue para a instalação C:\RSA e clique em **salve** e **fechar**.  
  
    9. Minimizar a **Console de segurança do RSA** para uso posterior.  
  
8.  Configure o Gerenciador de autenticação como servidor RADIUS.  
  
    1.  A RSA computador desktop clicar duas vezes **"Console de operações de segurança do RSA"** .  
  
    2.  Se a segurança de certificado aviso / alerta de segurança for exibida, clique em **continuar neste site** ou clique em **Sim** para prosseguir e adicionar este site aos sites confiáveis se solicitado.  
  
    3.  Insira a ID de usuário e senha e clique em **fazer logon**.  
  
    4.  Clique em **configuração de implantação - RADIUS - configurar servidor**.  
  
    5.  Sobre o **credenciais adicionais necessárias** página, insira a ID de usuário de administrador e a senha e clique em **Okey**.  
  
    6.  Sobre o **configurar o servidor RADIUS** a mesma senha usada para o usuário administrador para entrar na página o **segredos** e **senha mestra**. Insira a ID de usuário administrador e a senha e, em seguida, clique em **configurar**.  
  
    7.  Verifique a mensagem **'servidor RADIUS configurado com êxito'** é exibida. Clique em **Concluído**. Fechar o **Console de operações de RSA**.  
  
    8.  Volte para o **"Console de segurança do RSA"** .  
  
    9. Sobre o **RADIUS** guia, clique em **servidores RADIUS**. Verifique se que esse rsa.corp.contoso.com está listado.  
  
9. Configure o servidor RSA como cliente de autenticação RSA.  
  
    1.  Sobre o **RADIUS** , clique em **clientes RADIUS** e **adicionar novo**.  
  
    2.  Clique o **qualquer cliente de RADIUS** caixa de seleção.  
  
    3.  Digite uma senha forte de sua escolha na **segredo compartilhado** campo. Você usará posteriormente essa mesma senha ao configurar EDGE1 para a OTP.  
  
    4.  Deixe o **endereço IP** em branco o campo e o **Verifique / Model** entrada como **RADIUS Standard**.  
  
    5.  Clique em **salvar sem Agente RSA**.  
  
10. Crie os arquivos necessários para configurar EDGE1 como um agente de autenticação RSA.  
  
    1.  Sobre o **acesso** guia, realce **agentes de autenticação**e clique em **adicionar novo**.  
  
    2.  Tipo de **EDGE1** na **nome do host** campo e, em seguida, clique em **IP resolver**.  
  
    3.  Observe que o endereço IP para EDGE1 agora é exibido na **endereço IP** campo. Clique em **Salvar**.  
  
11. Gere um arquivo de configuração para o servidor EDGE1 (AM_Config.zip).  
  
    1.  Sobre o **acesso** guia, realce **agentes de autenticação**e clique em **gerar arquivo de configuração**.  
  
    2.  Sobre o **gerar arquivo de configuração** página, clique em **gerar arquivo de configuração**e, em seguida, clique em **baixar agora**.  
  
    3.  Clique em **salvar**, navegue até o C:\ Instalação de RSA e clique em **salvar**.  
  
    4.  Clique em **feche** sobre o **Download concluído** caixa de diálogo.  
  
12. Gere um arquivo de segredo de nó para o servidor EDGE1 (EDGE1_NodeSecret.zip).  
  
    1.  Sobre o **acesso** guia, realce **agentes de autenticação**e clique em **gerenciar existente**.  
  
    2.  Clique no nó atual configurado EDGE1 e, em seguida, clique em **segredo de nó gerenciar**.  
  
    3.  Verifique as **criar um novo segredo aleatório de nó e exportar o segredo de nó para um arquivo** caixa de seleção.  
  
    4.  Insira a mesma senha usada para o usuário administrador na **senha de criptografia** e **Confirmar senha de criptografia** campos e clique em **salvar**.  
  
    5.  Sobre o **nó segredo do arquivo gerado** página, clique em **baixar agora**.  
  
    6.  No **Download de arquivo** caixa de diálogo clique **salvar**, procure C:\RSA instalação e clique em **salvar**. Clique em **feche** sobre o **Download concluído** caixa de diálogo.  
  
    7.  Do que o Gerenciador de autenticação RSA mídia cópia \auth_mgr\windows-x86_64\am\rsa-ace_nsload\win32-5.0-x86\agent_nsload.exe à instalação C:\RSA.  
  
## <a name="BKMK_DAProbeUser"></a>Criar DAProbeUser  
  
1.  No **Console de segurança do RSA** clique a **identidade** , clique em **usuários**e clique em **adicionar novo**.  
  
2.  No **sobrenome:** tipo de seção **investigação**e, na **ID de usuário:** seção tipo **DAProbeUser**. No **senha:** e **Confirmar senha:** seções digite uma senha forte. Desmarque a **'Exigir que o usuário altere a senha no próximo logon'** caixa de seleção e clique em **salvar**.  
  
## <a name="InstToken"></a>Instalar o token de software RSA SecurID no CLIENT1  
Use este procedimento para instalar o token de software SecurID no CLIENT1.  
  
#### <a name="install-securid-software-token"></a>Instalar o token de software SecurID  
  
1.  No computador CLIENT1, crie a pasta arquivos de C:\RSA. Copie o arquivo Software_Tokens.zip de C:\RSA instalação no computador RSA C:\RSA arquivos. Extraia o arquivo User1_000031701832.SDTID C:\RSA arquivos no CLIENT1.  
  
2.  Acessar a fonte de mídia do token de software RSA SecurID e clique duas vezes em RSASECURIDTOKEN410 na **aplicativo de cliente SecurID SoftwareToken** pasta para iniciar a instalação do RSA SecurID. Se o **abrir arquivo – Aviso de segurança** mensagem será exibida, em seguida, clique em **executar**.  
  
3.  Sobre o **RSA SecurID Software Token - Assistente do InstallShield** caixa de diálogo clique **próxima** duas vezes.  
  
4.  Aceite o contrato de licença e, em seguida, clique em **próxima**.  
  
5.  Sobre o **o tipo de instalação** diálogo, selecione **típica**, clique em **próxima**e clique em **instalar**.  
  
6.  Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
7.  Selecione o **Software iniciar RSA SecurID Token** caixa de seleção e, em seguida, clique em **concluir**.  
  
8.  Clique em **importar do arquivo**.  
  
9. Clique em **navegue**, selecione C:\RSA Files\User1_000031701832.SDTID e clique em **abrir**.  
  
10. Clique em **OK** duas vezes.  
  
## <a name="configAuthAgt"></a>Configurar EDGE1 como um agente de autenticação RSA  
Use este procedimento para configurar EDGE1 para realizar a autenticação de RSA.  
  
#### <a name="configure-the-rsa-authentication-agent"></a>Configurar o agente de autenticação RSA  
  
1. Em EDGE1 abra o Windows Explorer e crie a pasta arquivos de C:\RSA. Navegue até a mídia de instalação do RSA ACE.  
  
2. Cópia agent_nsload.exe arquivos, AM_Config.zip e EDGE1_NodeSecret.zip da mídia de RSA para C:\RSA arquivos.  
  
3. Extraia o conteúdo dos dois arquivos zip nos seguintes locais:  
  
   1.  C:\Windows\system32\  
  
   2.  C:\Windows\SysWOW64\  
  
4. Copiar agent_nsload.exe para C:\Windows\SysWOW64\\.  
  
5. Abra um prompt de comando com privilégios elevados e navegue para C:\Windows\SysWOW64.  
  
6. Tipo de **nodesecret.rec do agent_nsload.exe -f -p <password>**  onde <password> é a senha forte que você criou durante a configuração inicial do RSA. Pressione Enter.  
  
7. Copie C:\Windows\SysWOW64\securid C:\Windows\System32.  
  
## <a name="configOTP"></a>Configurar EDGE1 para dar suporte à autenticação de OTP  
Use este procedimento para configurar a OTP do DirectAccess e verifique a configuração.  
  
#### <a name="configure-otp-for-directaccess"></a>Configurar a OTP do DirectAccess  
  
1.  Em EDGE1, abra o Gerenciador do servidor e clique em **acesso remoto** no painel esquerdo.  
  
2.  Clique com botão direito **EDGE1** no painel servidores e selecione **gerenciamento de acesso remoto**.  
  
3.  Clique em **configuração**.  
  
4.  No **instalação do DirectAccess** janela, em **etapa 2: servidor de acesso remoto**, clique em **editar**.  
  
5.  Clique em **próxima** três vezes e, nas **autenticação** seção select **autenticação de dois fatores** e **usar OTP**e certifique-se de que **Usar certificados de computador** é verificada. Verificar se a AC raiz é definida como **CN = corp-APP1-CA**. Clique em **Avançar**.  
  
6.  No **servidor do RADIUS OTP** seção, clique duas vezes o espaço em branco **nome do servidor** campo.  
  
7.  No **adicionar um servidor RADIUS** caixa de diálogo, digite **RSA** no **nome do servidor** campo. Clique em **alteração** lado a **segredo compartilhado** campo e, em seguida, digite a mesma senha que você usou ao configurar clientes RADIUS no servidor de RSA no **novo segredo** e  **Confirmar segredo novo** campos. Clique em **Okey** duas vezes e clique em **próxima**.  
  
    > [!NOTE]  
    > Se o servidor RADIUS estiver em um domínio diferente do servidor de acesso remoto, em seguida, a **nome do servidor** campo deve especificar o FQDN do servidor RADIUS.  
  
8.  No **servidores CA OTP** seção APP1.corp.contoso.com select e, em seguida, clique em **Add**. Clique em **Avançar**.  
  
9. Sobre o **modelos de certificado de OTP** página, clique em **procurar** para selecionar um modelo de certificado usado para o registro de certificados que são emitidos para autenticação OTP e, no  **Modelos de certificado** caixa de diálogo, selecione **DAOTPLogon**. Clique em **OK**. Clique em **navegue** para selecionar um modelo de certificado usado para registrar o certificado usado pelo servidor de acesso remoto para solicitações de registro de certificado OTP de entrada e nos **modelos de certificado** caixa de diálogo Selecione **DAOTPRA**. Clique em **Ok**. Clique em **Avançar**.  
  
10. No **configuração do servidor de acesso remoto** página, clique em **concluir**e clique em **concluir** no **Assistente do DirectAccess especialista**.  
  
11. Sobre o **revisão de acesso remoto** clique da caixa de diálogo **aplicar**, aguarde até que a diretiva do DirectAccess a ser atualizado e clique em **fechar**.  
  
12. Sobre o **iniciar** tela, digite**powershell.exe**, clique com botão direito **powershell**, clique em **avançado**e clique em **executar como administrador**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
13. Na janela do Windows PowerShell, digite **gpupdate /force** e pressione ENTER.  
  
14. Feche e reabra o Console de gerenciamento de acesso remoto e verificar se todas as configurações de OTP estão corretas.  
  


