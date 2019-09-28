---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: Guia de Implantação do AD FS do Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c549b52f697db889bf638470aca03f02e1323ed5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359730"
---
# <a name="configure-a-federation-server"></a>Configurar um servidor de federação

Depois de instalar o serviços de Federação do Active Directory (AD FS) \(AD FS\) serviço de função em seu computador, você estará pronto para configurar este computador para se tornar um servidor de Federação. Você pode executar um dos seguintes procedimentos:  
  
-   [Configurar o primeiro servidor de Federação em um novo farm de servidores de Federação](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [Adicionar um servidor de Federação a um farm de servidores de Federação existente](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>Configurar o primeiro servidor de Federação em um novo farm de servidores de Federação  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Para configurar o primeiro servidor de Federação em um novo farm de servidores de Federação usando o assistente de configuração do Active Directory Serviço de Federação  
  
> [!NOTE]  
> Verifique se você tem permissões de administrador de domínio ou tenha credenciais de administrador de domínio disponíveis antes de executar este procedimento.  
  
1.  Na página **Painel** do Gerenciador do Servidor, clique no sinalizador **Notificações** e, depois, clique em **Configurar o serviço de federação neste servidor**.  
  
    O **Assistente de Configuração do Serviço de Federação do Active Directory** é aberto.  
  
2.  Na página **Bem-vindo**, selecione **Criar o primeiro servidor de federação em um farm de servidores de federação** e clique em **Avançar**.  
  
3.  Na página **conectar a AD DS** , especifique uma conta usando permissões de administrador de domínio para o Active Directory \(domínio\) do AD ao qual este computador está associado e clique em **Avançar**.  
  
4.  Na página **Especificar Propriedades de Serviço** , siga estas etapas e clique em **Avançar**:  
  
    -   Importe o arquivo. pfx que contém o certificado SSL \(\) da camada de soquete seguro e a chave que você obteve anteriormente. Na [etapa 2: Registre um certificado SSL para AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), você obteve esse certificado e o copiou no computador que deseja configurar como um servidor de Federação. Para importar o arquivo. pfx por meio do assistente, clique em **importar**e navegue até o local do arquivo. Digite a senha para o arquivo. pfx quando solicitado.  
  
    -   Forneça um nome para o serviço de Federação. Por exemplo, **FS.contoso.com**. Esse nome deve corresponder a um dos nomes de assunto ou alternativo da entidade no certificado.  
  
    -   Forneça um nome de exibição para o serviço de Federação. Por exemplo, **Contoso Corporation**. Os usuários veem esse nome na página \(de\) entrada\-do serviços de Federação do Active Directory (AD FS) AD FS.  
  
5.  Na página **especificar conta de serviço** , especifique uma conta de serviço. Você pode criar ou usar uma conta \(de serviço gerenciado de grupo existente gMSA\) ou usar uma conta de usuário de domínio existente. Se você selecionar a opção para criar uma nova conta do gMSA, especifique um nome para a nova conta. Se você selecionar a opção para usar uma conta de domínio ou gMSA existente, clique em **selecionar** para selecionar uma conta.  
  
    > [!NOTE]  
    > O benefício de usar uma conta do gMSA é o\-recurso de atualização de senha negociada automaticamente.  
  
    > [!WARNING]  
    > Se você quiser usar uma conta do gMSA, deverá ter pelo menos um controlador de domínio em seu ambiente que esteja executando o sistema operacional Windows Server 2012.  
    >   
    > Se a opção gMSA estiver desabilitada e você vir uma mensagem de erro, como **contas de serviço gerenciado de grupo não estão disponíveis porque a chave raiz KDS não foi definida**, você poderá habilitar o gMSA em seu domínio executando o seguinte comando do Windows PowerShell em um domínio controlador, que executa o Windows Server 2012 ou posterior, em seu domínio de `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`Active Directory:. Em seguida, retorne ao assistente, clique em **anterior**e, em seguida\-, clique em **Avançar** para inserir novamente a página **especificar conta de serviço** . A opção gMSA agora deve estar habilitada. Você pode selecioná-lo e inserir um nome de conta do gMSA que você deseja usar.  
  
6.  Na página **especificar banco de dados de configuração** , especifique um banco de dados de configuração de AD FS e clique em **Avançar**. Você pode criar um banco de dados neste computador usando o wid \(\)do banco de dados interno do Windows ou pode especificar o local e o nome da instância de Microsoft SQL Server.  
  
    Para saber mais, confira [A função do banco de dados de configuração de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
    > [!IMPORTANT]  
    > Se você quiser criar um farm de AD FS e usar SQL Server para armazenar seus dados de configuração, poderá usar SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012 e SQL Server 2014.  
  
7.  Na página **Examinar Opções** , verifique suas seleções de configuração e clique em **Avançar**.  
  
8.  Na página **verificações\-de pré-requisitos** , verifique se todas as verificações de pré-requisitos foram concluídas com êxito e clique em **Configurar**.  
  
9. Na página **resultados** , examine os resultados e verifique se a configuração foi concluída com êxito e clique em **próximas etapas necessárias para concluir a implantação do serviço de Federação**. Para obter mais informações, consulte [próximas etapas para concluir a instalação do AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Clique em **Fechar** para sair do assistente.  
  
### <a name="BKMK_3"></a>Para configurar o primeiro servidor de Federação em um novo farm de servidores de Federação por meio do Windows PowerShell  
Você pode criar um novo farm de servidores de Federação usando uma conta do gMSA nova ou existente ou uma conta de usuário de domínio existente.  
  
-   **Se você quiser criar um novo servidor de Federação usando uma nova conta do gMSA, faça o seguinte:**  
  
    > [!IMPORTANT]  
    > Você deve ter permissões de administrador de domínio para criar o primeiro servidor de Federação em um farm de servidores de Federação novo.  
  
    1.  No computador que você deseja configurar como um servidor de Federação, verifique se o certificado SSL necessário foi importado para o **computador\\local meu** diretório de repositório. Você pode verificar se o certificado SSL foi importado executando o seguinte comando na janela de comando do Windows PowerShell `dir Cert:\LocalMachine\My`:. O certificado é listado por sua impressão digital no **computador\\local meu** diretório de repositório.  
  
    2.  No controlador de domínio, abra a janela de comando do Windows PowerShell e execute o seguinte comando para verificar se a chave raiz KDS foi criada em seu domínio `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`:. Se não tiver sido criado para que a saída não exiba nenhuma informação, execute o seguinte comando para criar a chave: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.  
  
    3.  No computador que você deseja configurar como um servidor de Federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > O `$` sinal no final do comando anterior é necessário.  
  
        Para obter o valor para `<certificate_thumbprint>`, execute `dir Cert:\LocalMachine\My`e, em seguida, selecione a impressão digital do seu certificado SSL. O valor de `<federation_service_name>` é o nome do seu serviço de Federação, por exemplo, **FS.contoso.com**.  
  
        > [!NOTE]  
        > Se essa não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um farm WID. Se desejar criar um farm de servidores SQL Server, você deverá ter uma instância do SQL Server já instalado e operacional.  
        >   
        > Você pode usar o comando a seguir para criar o primeiro servidor de Federação em um novo farm que usa uma instância de `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"` SQL Server: onde **<\_nome de host do SQL\_>** é o nome do servidor no qual SQL Server está em execução e **< nome\_da\_instância SQL >** é o nome da instância do SQL Server. Se você usar a instância padrão do SQL Server, use um valor **SqlConnectionString** de " **\=fonte de dados < nome\_do host SQL\_>;\=segurança integrada true**".  
  
        > [!IMPORTANT]  
        > Se você deseja criar um farm do AD FS e usar o SQL Server para armazenar seus dados de configuração, pode usar o SQL Server 2008 e versões mais recentes, incluindo o SQL Server 2012.  
  
-   **Se você quiser criar um novo servidor de Federação usando uma conta de usuário de domínio existente, faça o seguinte:**  
  
    1.  No computador que você deseja configurar como um servidor de Federação, verifique se o certificado SSL necessário foi importado para o **computador\\local meu** diretório de repositório. Você pode verificar se o certificado SSL foi importado executando o seguinte comando na janela de comando do Windows PowerShell `dir Cert:\LocalMachine\My`:. O certificado é listado por sua impressão digital no **computador\\local meu** diretório de repositório.  
  
    2.  No computador que você deseja configurar como um servidor de Federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando: `$fscred = Get-Credential`. Insira as credenciais de conta de usuário de domínio que você deseja usar para a conta de serviço de Federação\\no formato domínio nome de usuário.  
  
    3.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        Para obter o valor de **< impressão\_digital do certificado >** , execute `dir Cert:\LocalMachine\My`e, em seguida, selecione a impressão digital do seu certificado SSL. O valor de **< nome\_do\_serviço de Federação >** é o nome do seu serviço de Federação, por exemplo, FS.contoso.com.  
  
        > [!NOTE]  
        > Se essa não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um farm WID. Se você quiser criar um farm de SQL Server, você deve ter a instância do SQL Server já instalada e operacional.  
        >   
        > Você pode usar o seguinte comando para criar o primeiro servidor de Federação em um novo farm que usa uma instância de SQL Server `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` : **onde\_nome\_do host SQL** é o nome do servidor no qual SQL Server está em execução e **SQL nomeda\_instância é o nome da instância do SQL Server. \_** Se você usar a instância padrão do SQL Server, use um valor **SqlConnectionString** de " **\=fonte de dados < nome\_do host SQL\_>;\=segurança integrada true**".  
  
        > [!IMPORTANT]  
        > Se você quiser criar um farm de AD FS e usar SQL Server para armazenar seus dados de configuração, poderá usar SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012 e SQL Server 2014.  
  
## <a name="BKMK_2"></a>Adicionar um servidor de Federação a um farm de servidores de Federação existente  
  
> [!IMPORTANT]  
> Verifique se você concluiu [a etapa 3: Instale o serviço](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md)de função AD FS, antes de iniciar qualquer um dos procedimentos nesta seção.  
  
> [!IMPORTANT]  
> Verifique se você obteve um certificado de autenticação de servidor SSL válido antes de concluir este procedimento.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Para adicionar um servidor de Federação a um farm de servidores de Federação existente por meio do assistente de configuração do Active Directory Serviço de Federação  
  
1.  Na página **Painel** do Gerenciador do Servidor, clique no sinalizador **Notificações** e, depois, clique em **Configurar o serviço de federação neste servidor**.  
  
    O **Assistente de Configuração do Serviço de Federação do Active Directory** é aberto.  
  
2.  Na página de **boas-vindas** , selecione **Adicionar um servidor de Federação a um farm de servidores de Federação**e clique em **Avançar**.  
  
3.  Na página **conectar a AD DS** , especifique uma conta usando permissões de administrador de domínio para o domínio do AD ao qual este computador está associado e clique em **Avançar**.  
  
4.  Na página **especificar farm** , forneça o nome do servidor de Federação primário em um farm que usa wid ou especifique o nome do host do banco de dados e o nome da instância do banco de dados de um farm de servidores de Federação existente que usa SQL Server.  
  
    > [!WARNING]  
    > No Windows Server® 2012 R2, há uma solução alternativa para especificar a instância padrão do SQL Server. A solução alternativa é não usar a interface do usuário. Em vez disso, use as etapas em [para configurar o primeiro servidor de Federação em um novo farm de servidores de Federação por meio do Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).  
  
    > [!IMPORTANT]  
    > Se você deseja criar um farm do AD FS e usar o SQL Server para armazenar seus dados de configuração, pode usar o SQL Server 2008 e versões mais recentes, incluindo o SQL Server 2012.  
  
5.  Na página **especificar certificado SSL** , importe o arquivo. pfx que contém o certificado SSL e a chave que você obteve anteriormente. Esse é o certificado obrigatório de autenticação de serviço. Na [etapa 2: Registre um certificado SSL para AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), você obteve esse certificado e o copiou para o computador que deseja configurar como um servidor de Federação. Para importar o arquivo. pfx por meio do assistente, clique em **importar** e navegue até o local do arquivo. Digite a senha para o arquivo. pfx quando solicitado.  
  
6.  Na página **especificar conta de serviço** , especifique a mesma conta de serviço que você configurou quando criou o primeiro servidor de Federação no farm. Você pode usar uma conta de serviço gerenciado de grupo existente ou uma conta de usuário de domínio existente.  
  
    > [!IMPORTANT]  
    > A conta que você especificar deve ser a mesma conta que a conta que foi usada no servidor de Federação primário neste farm.  
  
7.  Na página **Examinar Opções** , verifique suas seleções de configuração e clique em **Avançar**.  
  
8.  Na página **verificações\-de pré-requisitos** , verifique se todas as verificações de pré-requisitos foram concluídas com êxito e clique em **Configurar**.  
  
9. Na página **resultados** , examine os resultados e verifique se a configuração foi concluída com êxito e clique em **próximas etapas necessárias para concluir a implantação do serviço de Federação**. Para obter mais informações, consulte [próximas etapas para concluir a instalação do AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Clique em **Fechar** para sair do assistente.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Para adicionar um servidor de Federação a um farm de servidores de Federação existente por meio do Windows PowerShell  
Você pode adicionar um servidor de Federação a um farm existente usando uma conta de gMSA existente ou uma conta de usuário de domínio existente.  
  
-   Se você quiser ingressar um servidor de Federação em um farm usando uma conta do gMSA existente, faça o seguinte:  
  
    1.  No computador que você deseja configurar como um servidor de Federação, verifique se o certificado SSL necessário foi importado para o **computador\\local meu** diretório de repositório. Você pode verificar se o certificado SSL foi importado executando o seguinte comando na janela de comando do Windows PowerShell `dir Cert:\LocalMachine\My`:. O certificado é listado por sua impressão digital no **computador\\local meu** diretório de repositório.  
  
    2.  No computador que você deseja configurar como um servidor de Federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>`é seu domínio do AD e o nome da sua conta do gMSA nesse domínio. `<first_federation_server_hostname>`é o nome do host do servidor de Federação primário neste farm existente.  
  
        Você pode obter o valor para `<certificate_thumbprint>` executando `dir Cert:\LocalMachine\My` na etapa anterior.  
  
        > [!NOTE]  
        > Se essa não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um nó de farm WID. Se você quiser criar um nó de farm de servidores de computadores que executam o SQL Server, você deve ter a instância do SQL Server já instalada e operacional.  
        >   
        > Você pode usar o seguinte comando para adicionar um servidor de Federação a um farm existente que está usando uma instância do SQL Server `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` : em que **nome do\_host\_SQL** é o nome do servidor no qual SQL Server está em execução e **SQL nomeda\_instância é o nome da instância do SQL Server. \_** Se você usar a instância padrão do SQL Server, use um valor **SqlConnectionString** de " **\=fonte de dados < nome\_do host SQL\_>;\=segurança integrada true**".  
  
        > [!IMPORTANT]  
        > Se você quiser criar um farm de AD FS e usar SQL Server para armazenar seus dados de configuração, poderá usar SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012 e SQL Server 2014.  
  
-   Se você quiser ingressar um servidor de Federação em um farm usando uma conta de usuário de domínio existente, faça o seguinte:  
  
    1.  No computador que você deseja configurar como um servidor de Federação, abra a janela PowerShellcommand do Windows e, em seguida, execute o seguinte `$fscred = get-credential`comando:. Insira as credenciais de conta de usuário de domínio que você deseja usar para a conta de serviço de Federação\\no formato domínio nome de usuário.  
  
    2.  No computador que você deseja configurar como um servidor de Federação, verifique se o certificado SSL necessário foi importado para o **computador\\local meu** diretório de repositório. Você pode verificar se o certificado SSL foi importado executando o seguinte comando na janela PowerShellcommand do Windows: `dir Cert:\LocalMachine\My`. O certificado é listado por sua impressão digital no **computador\\local meu** diretório de repositório.  
  
    3.  Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > Se essa não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um nó de farm WID. Se você quiser criar um nó de farm de servidores de computadores que executam o SQL Server, você deve ter a instância do SQL Server já instalada e operacional. Você pode usar o comando a seguir para adicionar um servidor de Federação a um farm existente usando uma instância do SQL Server `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` : em que **nome do host\_\_SQL** é o nome do servidor no qual a instância do SQL Server está em execução e **Nome\_dainstância\_SQL** é o nome da instância do SQL Server. Se você usar a instância padrão do SQL Server, use um valor **SqlConnectionString** de " **\=fonte de dados < nome\_do host SQL\_>;\=segurança integrada true**".  
  
        > [!IMPORTANT]  
        > Se você quiser criar um farm de AD FS e usar SQL Server para armazenar seus dados de configuração, poderá usar SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012 e SQL Server 2014.  
  
## <a name="see-also"></a>Consulte também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

