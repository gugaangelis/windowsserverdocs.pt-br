---
ms.assetid: 6b38480e-5b1c-49f0-9d46-8cf22f70f0d2
title: "Configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 049a1a1b0a419b0194edfe56b356a9f1e8b4b058
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="set-up-the-lab-environment-for-ad-fs-in-windows-server-2012-r2"></a>Configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2

>Aplica-se a: Windows Server 2012 R2

Este tópico descreve as etapas para configurar um ambiente de teste que pode ser usado para concluir a orientações passo a passo os seguintes guias passo a passo:

-   [Passo a passo: Workplace Join com um dispositivo iOS](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

-   [Passo a passo: Associação de local de trabalho com um dispositivo Windows](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)


-   [Guia passo a passo: Gerenciar o risco com controle de acesso condicional](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)

-   [Guia passo a passo: Gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

> [!NOTE]
> Não recomendamos que você instale o servidor web e o servidor de federação no mesmo computador.

Para configurar esse ambiente de teste, conclua as seguintes etapas:

1.  [Etapa 1: Configurar o controlador de domínio (DC1)](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_1)

2.  [Etapa 2: Configurar o servidor de Federação (ADFS1) com o serviço de registro de dispositivo](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)

3.  [Etapa 3: Configurar o servidor web (WebServ1) e um aplicativo de exemplo baseada em declarações](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_5)

4.  [Etapa 4: Configurar o computador cliente (Client1)](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_10)

## <a name="BKMK_1"></a>Etapa 1: Configurar o controlador de domínio (DC1)
Para os fins desse ambiente de teste, você pode chamar seu domínio do Active Directory raiz **contoso.com** e especifique ** pass@word1 ** como a senha de administrador.

-   Instale o serviço de função do AD DS e instalar os serviços de domínio do Active Directory (AD DS) para tornar seu computador um controlador de domínio no Windows Server 2012 R2. Essa ação atualiza seu esquema do AD DS como parte da criação de controlador de domínio. Para obter mais informações e instruções passo a passo, consulte[https://technet.microsoft.com/library/hh472162.aspx ](https://technet.microsoft.com/library/hh472162.aspx).

### <a name="BKMK_2"></a>Criar contas do Active Directory de teste
Depois que o controlador de domínio é funcional, você pode criar contas de usuário de grupo e teste um teste neste domínio e adicione a conta de usuário à conta de grupo. Você pode usar essas contas para concluir a orientações passo a passo os guias passo a passo que são abordados neste tópico.

Crie as contas a seguir:

-   Usuário: **Robert Hatley** com as seguintes credenciais: nome de usuário: **RobertH** e a senha:**P@ssword**

-   Grupo: **Finanças**

Para obter informações sobre como criar contas de usuário e grupo no Active Directory (AD), consulte [https://technet.microsoft.com/library/cc783323%28v.aspx ](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx).

Adicione o **Robert Hatley** conta para a **Finanças** grupo. Para obter informações sobre como adicionar um usuário a um grupo no Active Directory, consulte [https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx ](https://technet.microsoft.com/library/cc737130%28v=ws.10%29.aspx).

### <a name="create-a-gmsa-account"></a>Criar uma conta GMSA
A conta de conta de serviço gerenciado (GMSA) do grupo é necessária durante a instalação de serviços de Federação do Active Directory (AD FS) e configuração.

##### <a name="to-create-a-gmsa-account"></a>Para criar uma conta GMSA

1.  Abra uma janela de comando do Windows PowerShell e digite:

    ```
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    New-ADServiceAccount FsGmsa -DNSHostName adfs1.contoso.com -ServicePrincipalNames http/adfs1.contoso.com

    ```

## <a name="BKMK_4"></a>Etapa 2: Configurar o servidor de Federação (ADFS1) usando o serviço de registro de dispositivo
Para configurar outra máquina virtual, instale o Windows Server 2012 R2 e conectá-lo ao domínio **contoso.com**. Configurar o computador depois que ingressaram no domínio e, em seguida, vá para instalar e configurar a função AD FS.

Para obter um vídeo, consulte [Active Directory federação serviços série de vídeos: instalar um Farm de servidores do AD FS](https://technet.microsoft.com/video/dn469436).

### <a name="install-a-server-ssl-certificate"></a>Instalar um certificado de servidor SSL
Você deve instalar um certificado de camada de soquete segura (SSL) do servidor no servidor ADFS1 no repositório de computador local. O certificado deve ter os seguintes atributos:

-   Nome (CN) do assunto: adfs1.contoso.com

-   Nome alternativo (DNS) do assunto: adfs1.contoso.com

-   Nome alternativo (DNS) do assunto: enterpriseregistration.contoso.com

Para obter mais informações sobre como configurar certificados SSL, consulte [configurar SSL/TLS em um site no domínio com uma autoridade de certificação corporativa](https://social.technet.microsoft.com/wiki/contents/articles/12485.configure-ssltls-on-a-web-site-in-the-domain-with-an-enterprise-ca.aspx).

[Série de vídeos de serviços de Federação do Active Directory: Atualizar certificados](https://technet.microsoft.com/video/adfs-updating-certificates).

### <a name="install-the-ad-fs-server-role"></a>Instale a função de servidor do AD FS

##### <a name="to-install-the-federation-service-role-service"></a>Para instalar o serviço de função do serviço de Federação

1.  Faça logon no servidor usando a conta de administrador do domínio administrator@contoso.com.

2.  Inicie o Gerenciador do servidor. Para iniciar o Gerenciador do servidor, clique em **Gerenciador do servidor** no Windows **iniciar** de tela ou clique em **Gerenciador do servidor** da barra de tarefas do Windows na área de trabalho do Windows. No **início rápido** guia do **boas-vindas** bloco no **painel** página, clique em **adicionar funções e recursos**. Como alternativa, você pode clicar em **adicionar funções e recursos** sobre o **gerenciar** menu.

3.  Sobre o **antes de começar** página, clique em **próxima**.

4.  No **selecionar o tipo de instalação** página, clique em **instalação baseada em função ou recurso baseado**e clique em **próxima**.

5.  No **servidor de destino Select** página, clique em **selecionar um servidor do pool servidor**, verifique se o computador de destino e clique em **próxima**.

6.  Sobre o **selecionar funções de servidor** página, clique em **serviços de Federação do Active Directory**e, em seguida, clique em **próxima**.

7.  Sobre o **Selecione recursos** página, clique em **próxima**.

8.  Sobre o **serviço de Federação do Active Directory (AD FS)** página, clique em **próxima**.

9. Depois de verificar as informações sobre o **confirmar seleções de instalação** página, selecione o **reiniciar o servidor de destino automaticamente, se necessário** caixa de seleção e, em seguida, clique em **instalar**.

10. Sobre o **progresso da instalação** página, verificar se tudo está instalado corretamente e, em seguida, clique em **fechar**.

### <a name="configure-the-federation-server"></a>Configurar o servidor de Federação
A próxima etapa é configurar o servidor de Federação.

##### <a name="to-configure-the-federation-server"></a>Para configurar o servidor de Federação

1.  No Gerenciador do servidor **painel** página, clique no **notificações** sinalizar e, em seguida, clique em **configurar o serviço de federação no servidor**.

    O **Assistente de configuração do serviço do Active Directory federação** é aberta.

2.  Sobre o **boas-vindas** página, selecione **criar o primeiro servidor de Federação em um farm de servidores de Federação**e clique em **próxima**.

3.  No **conectar no AD DS** página, especifique uma conta com direitos de administrador do domínio para o **contoso.com** domínio do Active Directory que ingressou neste computador e clique **próxima**.

4.  Sobre o **especificar propriedades do serviço** de página, faça o seguinte e, em seguida, clique em **próxima**:

    -   Importe o certificado SSL tiver obtido anteriormente. Esse certificado é o certificado de autenticação de serviço necessárias. Navegue até o local do seu certificado SSL.

    -   Para fornecer um nome para seu serviço de federação, digite **adfs1.contoso.com**. Esse valor é o mesmo valor que você forneceu quando você tiver registrado um certificado SSL nos serviços de certificados do Active Directory (AD CS).

    -   Para fornecer um nome de exibição para seu serviço de federação, digite **Contoso Corporation**.

5.  Sobre o **especificar a conta de serviço** página, selecione **usar uma conta de usuário de domínio existente ou grupo de conta de serviço gerenciado**e, em seguida, especifique a conta GMSA **fsgmsa** que você criou ao criar o controlador de domínio.

6.  Sobre o **especificar banco de dados de configuração** página, selecione **criar um banco de dados no servidor usando o banco de dados interno do Windows**e clique em **próxima**.

7.  Sobre o **opções de revisão** página, verifique se suas seleções de configuração e, em seguida, clique em **próxima**.

8.  Sobre o **pré-requisito verifica** página, verifique se todas as verificações de pré-requisito foram concluídas com êxito e, em seguida, clique em **configurar**.

9. Sobre o **resultados** página, examinar os resultados, verifique se a configuração foi concluída com êxito e, em seguida, clique em **próximas etapas necessárias para concluir a implantação do serviço de Federação**.

### <a name="configure-device-registration-service"></a>Configurar o serviço de registro de dispositivo
A próxima etapa é configurar o serviço de registro de dispositivo no servidor ADFS1. Para obter um vídeo, consulte [Active Directory federação serviços série de vídeos: habilitar o serviço de registro de dispositivo](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service).

##### <a name="to-configure-device-registration-service-for-windows-server-2012-rtm"></a>Para configurar o dispositivo de registro de serviço para o Windows Server 2012 RTM

1.  > [!IMPORTANT]
    > **A etapa a seguir aplica-se para a compilação do Windows Server 2012 R2 RTM.**

    Abra uma janela de comando do Windows PowerShell e digite:

    ```
    Initialize-ADDeviceRegistration
    ```

    Quando você for solicitado para uma conta de serviço, digite **contoso\fsgmsa$**.

    Agora execute o cmdlet do Windows PowerShell.

    ```
    Enable-AdfsDeviceRegistration
    ```

2.  No servidor ADFS1, no **AD FS gerenciamento** do console, navegue até **políticas de autenticação**. Selecione **Editar autenticação primária Global**. Marque a caixa de seleção ao lado de **habilitar a autenticação de dispositivo**e clique em **Okey**.

### <a name="add-host-a-and-alias-cname-resource-records-to-dns"></a>Adicionar Host (A) e registros de recurso de Alias (CNAME) ao DNS
Em DC1, você deve garantir que os seguintes registros de sistema de nome de domínio (DNS) são criados para o serviço de registro do dispositivo.

|Entrada|Tipo|Endereço|
|---------|--------|-----------|
|adfs1|Host (A)|Endereço IP do servidor do AD FS|
|enterpriseregistration|Alias (CNAME)|adfs1.contoso.com|

Você pode usar o procedimento a seguir para adicionar um registro de recurso de host (A) aos servidores corporativos de nome DNS para o servidor de Federação e o serviço de registro do dispositivo.

A associação ao grupo Administradores ou seu equivalente é o requisito mínimo para concluir este procedimento. Examine os detalhes sobre como usar as contas apropriadas e associações de grupo no hiperlink "https://go.microsoft.com/fwlink/?LinkId=83477" Local e grupos de domínio padrão (https://go.microsoft.com/fwlink/p/?LinkId=83477).

##### <a name="to-add-a-host-a-and-alias-cname-resource-records-to-dns-for-your-federation-server"></a>Adicionar um host (A) e registros de recurso de alias (CNAME) para o DNS para o servidor de Federação

1.  Em DC1, do Gerenciador do servidor, no **ferramentas** menu, clique em **DNS** para abrir o snap-in do DNS.

2.  Na árvore de console, expanda DC1, **zonas de pesquisa direta**, clique com botão direito **contoso.com**e clique em **novo Host (A ou AAAA)**.

3.  Em **nome,** digite o nome que você deseja usar para seu farm AD FS. Para este passo a passo, digite **adfs1**.

4.  Em **endereço IP**, digite o endereço IP do servidor ADFS1. Clique em **Adicionar Host**.

5.  Clique com botão direito **contoso.com**e clique em **novo Alias (CNAME)**.

6.  No **novo registro de recurso** caixa de diálogo, digite **enterpriseregistration** no **apelido** caixa.

7.  No FQDN domínio totalmente qualificado nome () da caixa de host de destino, digite **adfs1.contoso.com**e clique em **Okey**.

    > [!IMPORTANT]
    > Em uma implantação do mundo real, se sua empresa tiver vários sufixos UPN (UPN), você deve criar vários registros CNAME, um para cada uma dessas sufixos UPN no DNS.

## <a name="BKMK_5"></a>Etapa 3: Configurar o servidor web (WebServ1) e um aplicativo de exemplo baseada em declarações
Configurar uma máquina virtual (WebServ1) ao instalar o sistema operacional Windows Server 2012 R2 e conectá-lo ao domínio **contoso.com**. Depois que ele ingressar no domínio, você poderá instalar e configurar a função servidor Web.

Para concluir o tutorial mencionado anteriormente neste tópico, você deve ter um aplicativo de exemplo que é protegido por seu servidor de Federação (ADFS1).

Você pode baixar o SDK do Windows Identity Foundation ([https://www.microsoft.com/download/details.aspx?id=4451](https://www.microsoft.com/download/details.aspx?id=4451), que inclui um aplicativo de exemplo baseada em declarações.

Você deve concluir as etapas a seguir para configurar um servidor web com essa amostra de aplicativo baseada em declarações.

> [!NOTE]
> Estas etapas foram testadas em um servidor web que executa o sistema operacional Windows Server 2012 R2.

1.  [Instalar a função servidor Web e o Windows Identity Foundation](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_15)

2.  [Instalar o Windows Identity Foundation SDK](../../ad-fs/deployment/../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_13)

3.  [Configurar o aplicativo de declarações simples no IIS](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_9)

4.  [Criar uma relação de confiança de terceiros confiante em seu servidor de Federação](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_11)

### <a name="BKMK_15"></a>Instale a função servidor Web e o Windows Identity Foundation

1.  > [!NOTE]
    > Você deve ter acesso à mídia de instalação do Windows Server 2012 R2.

    Fazer logon em WebServ1 usando ** administrator@contoso.com ** e a senha ** pass@word1 **.

2.  No Gerenciador de servidores, no **início rápido** guia do **boas-vindas** bloco no **painel** página, clique em **adicionar funções e recursos**. Como alternativa, você pode clicar em **adicionar funções e recursos** sobre o **gerenciar** menu.

3.  Sobre o **antes de começar** página, clique em **próxima**.

4.  No **selecionar o tipo de instalação** página, clique em **instalação baseada em função ou recurso baseado**e clique em **próxima**.

5.  No **servidor de destino Select** página, clique em **selecionar um servidor do pool servidor**, verifique se o computador de destino e clique em **próxima**.

6.  Sobre o **selecionar funções de servidor** página, marque a caixa de seleção ao lado **Web Server (IIS)**, clique em **adicionar recursos**e, em seguida, clique em **próxima**.

7.  Sobre o **Selecione recursos** , selecione **Windows Identity Foundation 3.5**e clique em **próxima**.

8.  Sobre o **função Web Server (IIS)** página, clique em **próxima**.

9. Sobre o **selecione Serviços de função** de página, selecione e expanda **desenvolvimento de aplicativos**. Selecione **ASP.NET 3.5**, clique em **adicionar recursos**e clique em **próxima**.

10. Sobre o **confirmar seleções de instalação** página, clique em **especificar um caminho de origem alternativo**. Insira o caminho para o diretório Sxs que está localizado na mídia de instalação do Windows Server 2012 R2. Por exemplo, D:\Sources\Sxs. Clique em **Okey**e clique em **instalar**.

### <a name="BKMK_13"></a>Instalar o Windows Identity Foundation SDK

1.  Execute WindowsIdentityFoundation-SDK-3.5.msi para instalar o Windows Identity Foundation SDK 3.5 (https://www.microsoft.com/download/details.aspx?id=4451). Escolha todas as opções padrão.

### <a name="BKMK_9"></a>Configurar o aplicativo de declarações simples no IIS

1.  Instale um certificado válido de SSL no repositório de certificados do computador. O certificado deve conter o nome do seu servidor web, **webserv1.contoso.com**.

2.  Copie o conteúdo do C:\Program Files (x86) \Windows Identity Foundation SDK\v3.5\Samples\Quick Start\Web Application\PassiveRedirectBasedClaimsAwareWebApp para C:\Inetpub\Claimapp.

3.  Edite o **Default.aspx.cs** para que nenhuma declaração filtragem assume o lugar do arquivo. Esta etapa é executada para garantir que o aplicativo de exemplo exibe todas as declarações são emitidas pelo servidor de Federação. Faça o seguinte:

    1.  Abrir **Default.aspx.cs** em um editor de texto.

    2.  Procure o arquivo para a segunda instância da `ExpectedClaims`.

    3.  Comente todo o `IF` declaração e suas chaves. Indicar comentários digitando "/ /" (sem aspas) no início de uma linha.

    4.  Seu `FOREACH` declaração agora deve se parecer com este exemplo de código.

        ```
        Foreach (claim claim in claimsIdentity.Claims)
        {
           //Before showing the claims validate that this is an expected claim
           //If it is not in the expected claims list then don't show it
           //if (ExpectedClaims.Contains( claim.ClaimType ) )
           // {
              writeClaim( claim, table );
           //}
        }

        ```

    5.  Salve e feche **Default.aspx.cs**.

    6.  Abrir **Web. config** em um editor de texto.

    7.  Remover todo o `<microsoft.identityModel>` seção. Remover tudo desde `including <microsoft.identityModel>` e até e incluindo `</microsoft.identityModel>`.

    8.  Salve e feche **Web. config**.

4.  **Configurar o Gerenciador do IIS**

    1.  Abrir **serviços de informações da Internet (IIS) Manager**.

    2.  Vá para **Pools de aplicativos**, clique com botão direito **DefaultAppPool** selecionar **configurações avançadas**. Defina **perfil de usuário carregar** para **True**e clique em **Okey**.

    3.  Clique com botão direito **DefaultAppPool** selecionar **configurações básicas**. Alterar o **.NET CLR versão** para **CLR .NET versão v2.0.50727**.

    4.  Clique com botão direito **Default Web Site** selecionar **Editar vinculações**.

    5.  Adicione um **HTTPS** associando a porta **443** com o certificado SSL que você instalou.

    6.  Clique com botão direito **Default Web Site** selecionar **Add Application**.

    7.  Defina o alias como **claimapp** e o caminho físico para **c:\inetpub\claimapp**.

5.  Para configurar **claimapp** para trabalhar com o servidor de federação, faça o seguinte:

    1.  Execute FedUtil.exe, que está localizado na **C:\Program Files (x86) \Windows Identity Foundation SDK\v3.5**.

    2.  Defina o local de configuração do aplicativo para **C:\inetput\claimapp\web.config** e defina o URI do aplicativo para a URL para o seu site, **https://webserv1.contoso.com /claimapp/**. Clique em **próxima**.

    3.  Selecione **usar um STS existente** e navegue até a URL de metadados do servidor do AD FS **https://adfs1.contoso.com/federationmetadata/2007-06/federationmetadata.xml**. Clique em **próxima**.

    4.  Selecione **desabilitar a validação da cadeia de certificados**e clique em **próxima**.

    5.  Selecione **sem criptografia**e clique em **próxima**. Sobre o **oferecidos requerimentos judiciais ou Extrajudiciais** página, clique em **próxima**.

    6.  Marque a caixa de seleção ao lado de **agendar uma tarefa para realizar atualizações de metadados de Federação WS diárias**. Clique em **concluir **.

    7.  Seu aplicativo de amostra agora está configurado. Se você testar a URL do aplicativo **https://webserv1.contoso.com/claimapp**, ele deve redirecioná-lo para o servidor de Federação. O servidor de federação deve exibir uma página de erro porque você ainda não tiver configurado a terceira confiança de terceiros. Em outras palavras, esse aplicativo de teste pelo AD FS não têm protegido.

Agora você deve proteger seu aplicativo de amostra que é executado em seu servidor web com o AD FS. Você pode fazer isso adicionando uma terceira confiança de terceiros em seu servidor de Federação (ADFS1). Para obter um vídeo, consulte [Active Directory federação serviços série de vídeos: adicionar uma dependência terceiros confiar em](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust).

### <a name="BKMK_11"></a>Criar uma relação de confiança de terceiros confiante em seu servidor de Federação

1.  Em você servidor de Federação (ADFS1), na **console de gerenciamento do AD FS**, navegue até **confie dependência de terceiros**e clique em **Adicionar dependência terceiros confiar**.

2.  No **Selecionar fonte de dados** , selecione **importar dados sobre o terceiro publicados online ou em uma rede local**, insira a URL de metadados para **claimapp**e clique em **próxima**. Executar FedUtil.exe criado um arquivo. XML de metadados. Ele está localizado em **https://webserv1.contoso.com/claimapp/federationmetadata/2007-06/federationmetadata.xml**.

3.  No **especificar o nome de exibição** página, especifique o **nome de exibição** para sua confiança confiante de terceiros, **claimapp**e clique em **próxima**.

4.  Sobre o **configurar a autenticação multifator agora? ** página, selecione **não quiser especificar a configuração de autenticação multifator para esta relação de confiança de terceiros terceira neste momento**e clique em **próxima**.

5.  Sobre o **escolher regras de autorização de emissão** página, selecione **permitir que todos os usuários acessem esse terceiro**e, em seguida, clique em **próxima**.

6.  Sobre o **pronto para adicionar confiar** página, clique em **próxima**.

7.  Sobre o **editar regras de declaração** caixa de diálogo, clique em **Adicionar regra**.

8.  Sobre o **escolher o tipo de regra** página, selecione **enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada**e, em seguida, clique em **próxima**.

9. No **configurar reivindicação regra** página, o **nome da regra reivindicação** , digite **todas as reclamações**. No **regra personalizada** , digite a seguinte regra reivindicação.

    ```
    c:[ ]
    => issue(claim = c);

    ```

10. Clique em **concluir**e clique em **Okey**.

## <a name="BKMK_10"></a>Etapa 4: Configurar o computador cliente (Client1)
Instalar o Windows 8.1 e configurar outra máquina virtual. Esta máquina virtual devem estar na mesma rede como as outras máquinas virtual. Este computador não deve ser ingressou no domínio Contoso.

O cliente deve confiar no certificado SSL que é usado para o servidor de Federação (ADFS1), que você configurou no [etapa 2: configurar o servidor de Federação (ADFS1) com o serviço de registro de dispositivo](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). Ele também deve ser capaz de validar informações de revogação de certificados para o certificado.

Você também deve configurar e usar uma conta da Microsoft para fazer logon no Client1.

## <a name="see-also"></a>Consulte também


- [No Active Directory federação serviços série de vídeos: Instalar um Farm de servidores de FS AD](https://technet.microsoft.com/video/dn469436)
- [Série de vídeos de serviços de Federação do Active Directory: Atualizar certificados](https://technet.microsoft.com/video/adfs-updating-certificates)
- [No Active Directory federação serviços série de vídeos: Adicione uma terceira confiança de terceiros](https://technet.microsoft.com/video/adfs-how-to-add-a-relying-party-trust)
- [Série de vídeos de serviços de Federação do Active Directory: Habilitar o serviço de registro do dispositivo](https://technet.microsoft.com/video/adfs-how-to-enabling-the-device-registration-service)
- [Série de vídeos de serviços de Federação do Active Directory: Instalar o Proxy de aplicativo Web](https://technet.microsoft.com/video/dn469438)



