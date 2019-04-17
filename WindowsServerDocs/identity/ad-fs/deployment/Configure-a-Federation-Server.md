---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: "Guia de implantação do Windows Server 2012 R2 AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13ce514dc5f3f70217a26c898cde6fe24d4967c6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-federation-server"></a>Configurar um servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Depois de instalar o serviço de função Serviços de Federação do Active Directory \(AD FS\) em seu computador, você estará pronto para configurar este computador para se tornar um servidor de Federação. Você pode fazer um destes procedimentos:  
  
-   [Configurar o servidor de Federação primeiro em um novo farm de servidores de Federação](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [Adicionar um servidor de Federação a um farm de servidor de Federação existente](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>Configurar o servidor de Federação primeiro em um novo farm de servidores de Federação  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Para configurar o servidor de Federação primeiro em um novo farm de servidores de Federação usando o Assistente de configuração do serviço de Federação Active Directory  
  
> [!NOTE]  
> Certifique-se de que você tem permissões de administrador do domínio ou ter credenciais de administrador de domínio disponíveis antes de executar este procedimento.  
  
1.  No Gerenciador do servidor **painel** página, clique no **notificações** sinalizar e, em seguida, clique em **configurar o serviço de federação no servidor**.  
  
    O **Assistente de configuração do serviço do Active Directory federação** é aberta.  
  
2.  Sobre o **boas-vindas** página, selecione **criar o primeiro servidor de Federação em um farm de servidores de Federação**e clique em **próxima**.  
  
3.  No **conectar no AD DS** página, especifique uma conta usando permissões de administrador do domínio para o domínio do Active Directory \(AD\) ao qual este computador está associado e clique **próxima**.  
  
4.  Sobre o **especificar propriedades do serviço** de página, faça o seguinte e, em seguida, clique em **próxima**:  
  
    -   Importe o arquivo. pfx que contenha o certificado SSL \(SSL\) e a chave que você obteve anteriormente. Em [etapa 2: registrar um certificado SSL do AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), você tiver obtido esse certificado e copiá-la no computador que você deseja configurar como um servidor de Federação. Para importar o arquivo. pfx por meio do assistente, clique em **importar**e, em seguida, navegue até o local do arquivo. Insira a senha para o arquivo. pfx quando for solicitado.  
  
    -   Forneça um nome para seu serviço de Federação. Por exemplo, **fs.contoso.com**. Esse nome deve coincidir com um dos nomes de assunto alternativos no certificado ou assunto.  
  
    -   Forneça um nome de exibição para seu serviço de Federação. Por exemplo, **Contoso Corporation**. Os usuários veem esse nome em \(AD FS\) sign\ os serviços de Federação do Active Directory-na página.  
  
5.  Sobre o **especificar a conta de serviço** página, especifique uma conta de serviço. Você pode criar ou usar um existente grupo conta de serviço gerenciado \(gMSA\) ou usar uma conta de usuário de domínio existente. Se você selecionar a opção de criar uma nova conta gMSA, especifique um nome para a nova conta. Se você selecionar a opção de usar uma conta de domínio ou gMSA existente, clique em **selecione** para selecionar uma conta.  
  
    > [!NOTE]  
    > A vantagem de usar uma conta de gMSA é o recurso de atualização negociada auto\ senha.  
  
    > [!WARNING]  
    > Se você quiser usar uma conta de gMSA, você deve ter pelo menos um controlador de domínio em seu ambiente que está executando o sistema operacional Windows Server 2012.  
    >   
    > Se a opção gMSA está desabilitada, e você vê uma mensagem de erro, como **contas de serviço gerenciado do grupo não estão disponíveis porque a chave de raiz KDS não tiver sido definida**, você pode habilitar gMSA em seu domínio executando o seguinte comando do Windows PowerShell em um controlador de domínio que executa o Windows Server 2012 ou posterior, no domínio do Active Directory:`Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Volte para o assistente, clique em **anterior**e clique em **próxima** para re\-insira o **especificar a conta de serviço** página. A opção gMSA agora deve ser habilitada. Você pode selecioná-lo e insira um nome de conta gMSA que você deseja usar.  
  
6.  Sobre o **especificar banco de dados de configuração** página, especificar um banco de dados de configuração do AD FS e, em seguida, clique em **próxima**. Você pode criar um banco de dados neste computador usando o banco de dados interno do Windows \(WID\), ou você pode especificar o local e o nome da instância do Microsoft SQL Server.  
  
    Para obter mais informações, consulte [a função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
    > [!IMPORTANT]  
    > Se você quiser criar um farm AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012 e o SQL Server 2014.  
  
7.  Sobre o **opções de revisão** página, verifique se suas seleções de configuração e, em seguida, clique em **próxima**.  
  
8.  Sobre o **Pre\ requisito verificações** página, verifique se todas as verificações de pré-requisito são concluídas com êxito e, em seguida, clique em **configurar**.  
  
9. Sobre o **resultados** página, examinar os resultados e verifique se a configuração for concluída com êxito e, em seguida, clique em **próximas etapas necessárias para concluir a implantação do serviço de Federação**. Para obter mais informações, consulte [próximas etapas para concluir a instalação do AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Clique em **fechar** para sair do assistente.  
  
### <a name="BKMK_3"></a>Para configurar o servidor de Federação primeiro em um novo farm de servidores de federação por meio do Windows PowerShell  
Você pode criar um novo farm de servidores de Federação usando uma conta nova ou existente gMSA ou uma conta de usuário de domínio existente.  
  
-   **Se você quiser criar um novo servidor de Federação usando uma nova conta gMSA, faça o seguinte:**  
  
    > [!IMPORTANT]  
    > Você deve ter permissões de administrador do domínio para criar o primeiro servidor de Federação em um novo farm de servidores de Federação.  
  
    1.  No computador em que você deseja configurar como um servidor de federação, certifique-se de que o certificado SSL necessário foi importado para o **repositório Local de computador\\Meus** diretório. Você pode verificar se o certificado SSL tiver sido importado, executando o comando a seguir na janela de comando do Windows PowerShell:`dir Cert:\LocalMachine\My`. O certificado é listado com sua impressão digital no **repositório Local de computador\\Meus** diretório.  
  
    2.  No controlador de domínio, abra a janela de comando do Windows PowerShell e execute o seguinte comando para verificar se a chave de raiz KDS foi criada em seu domínio:`Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Se não tiver sido criado para que a saída não exibe nenhuma informação, execute o seguinte comando para criar a chave:`Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.  
  
    3.  No computador em que você deseja configurar como um servidor de federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > O `$`entrada no final do comando anterior é necessária.  
  
        Para obter o valor para `<certificate_thumbprint>`, execução `dir Cert:\LocalMachine\My`e, em seguida, selecione a impressão digital do seu certificado SSL. O valor de `<federation_service_name>` é o nome do seu serviço de federação, por exemplo, **fs.contoso.com**.  
  
        > [!NOTE]  
        > Se isso não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um farm de trabalho. Se você quiser criar um farm de servidores do SQL Server, você deve ter uma instância do SQL Server já instalado e em operação.  
        >   
        > Você pode usar o seguinte comando para criar o primeiro servidor de Federação em um novo farm que usa uma instância do SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"` onde **< SQL\_Host\_Name >** é o nome do servidor no qual está executando o SQL Server, e **< SQL\_instance\_name >** é o nome da instância do SQL Server. Se você usar a instância padrão do SQL Server, use um **sessionState** valor de "**dados Source\ = < SQL\_Host\_Name >; integrado Security\ = True**".  
  
        > [!IMPORTANT]  
        > Se você quiser criar um farm AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012.  
  
-   **Se você quiser criar um novo servidor de Federação usando uma conta de usuário de domínio existente, faça o seguinte:**  
  
    1.  No computador em que você deseja configurar como um servidor de federação, certifique-se de que o certificado SSL necessário foi importado para o **repositório Local de computador\\Meus** diretório. Você pode verificar se o certificado SSL tiver sido importado, executando o comando a seguir na janela de comando do Windows PowerShell:`dir Cert:\LocalMachine\My`. O certificado é listado com sua impressão digital no **repositório Local de computador\\Meus** diretório.  
  
    2.  No computador em que você deseja configurar como um servidor de federação, abra a janela de comando do Windows PowerShell e, em seguida, execute o seguinte comando: `$fscred = Get-Credential`. Insira as credenciais de conta de usuário de domínio que você deseja usar para a conta de serviço de federação no formato domínio \ \ usuário nome.  
  
    3.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        Para obter o valor para **< certificate\_thumbprint >**, execução `dir Cert:\LocalMachine\My`e, em seguida, selecione a impressão digital do seu certificado SSL. O valor de **< federation\_service\_name >** é o nome do seu serviço de federação, por exemplo, fs.contoso.com.  
  
        > [!NOTE]  
        > Se isso não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um farm de trabalho. Se você quiser criar um farm de SQL Server, você deve ter a instância do SQL Server já instalado e em operação.  
        >   
        > Você pode usar o seguinte comando para criar o primeiro servidor de Federação em um novo farm que usa uma instância do SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` onde **SQL\_Host\_Name** é o nome do servidor no qual está executando o SQL Server, e **SQL\_instance\_name** é o nome da instância do SQL Server. Se você usar a instância padrão do SQL Server, use um **sessionState** valor de "**dados Source\ = < SQL\_Host\_Name >; integrado Security\ = True**".  
  
        > [!IMPORTANT]  
        > Se você quiser criar um farm AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012 e o SQL Server 2014.  
  
## <a name="BKMK_2"></a>Adicionar um servidor de Federação a um farm de servidor de Federação existente  
  
> [!IMPORTANT]  
> Certifique-se de que você tenha concluído [etapa 3: instalar o serviço de função do AD FS](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md), antes de iniciar qualquer um dos procedimentos desta seção.  
  
> [!IMPORTANT]  
> Certifique-se de que você obteve um servidor SSL válido certificado de autenticação antes de concluir este procedimento.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Para adicionar um servidor de Federação a um farm de servidores de Federação existente via o Assistente de configuração do serviço de Federação Active Directory  
  
1.  No Gerenciador do servidor **painel** página, clique no **notificações** sinalizar e, em seguida, clique em **configurar o serviço de federação no servidor**.  
  
    O **Assistente de configuração do serviço do Active Directory federação** é aberta.  
  
2.  Sobre o **boas-vindas** página, selecione **adicionar um servidor de federação para um farm de servidores de Federação**e clique em **próxima**.  
  
3.  Sobre o **conectar no AD DS** página, especifique uma conta usando permissões de administrador do domínio para o domínio do AD para o qual este computador está associado e clique **próxima**.  
  
4.  Sobre o **especificar Farm** página, forneça o nome do servidor de Federação principal em um farm que usa o trabalho ou especificar o nome de host do banco de dados e o nome da instância de banco de dados de um farm de servidores de Federação existente que usa o SQL Server.  
  
    > [!WARNING]  
    > No Windows Server® 2012 R2, há uma solução alternativa para especificar a instância padrão do SQL Server. A solução alternativa é não usar a interface do usuário. Em vez disso, use as etapas em [para configurar o servidor de Federação primeiro em um novo farm de servidores de federação por meio do Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).  
  
    > [!IMPORTANT]  
    > Se você quiser criar um farm AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012.  
  
5.  Sobre o **especificar o certificado SSL** página, importe o arquivo. pfx que contenha o certificado SSL e a chave que você obteve anteriormente. Esse certificado é o certificado de autenticação de serviço necessárias. Em [etapa 2: registrar um certificado SSL do AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), você tiver obtido esse certificado e copiá-lo para o computador que você deseja configurar como um servidor de Federação. Para importar o arquivo. pfx por meio do assistente, clique em **importar** e navegue até o local do arquivo. Insira a senha para o arquivo. pfx quando for solicitado.  
  
6.  Sobre o **especificar a conta de serviço** página, especificar a mesma conta de serviço que você configurou quando criou o primeiro servidor de federação no farm. Você pode usar um conta de serviço gerenciado do grupo existente ou uma conta de usuário de domínio existente.  
  
    > [!IMPORTANT]  
    > A conta que você especificar deve ser a mesma conta da conta que foi usada no servidor de Federação principal neste farm.  
  
7.  Sobre o **opções de revisão** página, verifique se suas seleções de configuração e, em seguida, clique em **próxima**.  
  
8.  Sobre o **Pre\ requisito verificações** página, verifique se todas as verificações de pré-requisito são concluídas com êxito e, em seguida, clique em **configurar**.  
  
9. Sobre o **resultados** página, examinar os resultados e verifique se a configuração for concluída com êxito e, em seguida, clique em **próximas etapas necessárias para concluir a implantação do serviço de Federação**. Para obter mais informações, consulte [próximas etapas para concluir a instalação do AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Clique em **fechar** para sair do assistente.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Para adicionar um servidor de Federação a um farm de servidores de Federação existente por meio do Windows PowerShell  
Você pode adicionar um servidor de Federação a um farm existente usando uma conta existente do gMSA ou uma conta de usuário de domínio existente.  
  
-   Se você deseja associar a um servidor de Federação a um farm usando uma conta existente do gMSA, faça o seguinte:  
  
    1.  No computador em que você deseja configurar como um servidor de federação, certifique-se de que o certificado SSL necessário foi importado para o **repositório Local de computador\\Meus** diretório. Você pode verificar se o certificado SSL tiver sido importado, executando o comando a seguir na janela de comando do Windows PowerShell:`dir Cert:\LocalMachine\My`. O certificado é listado com sua impressão digital no **repositório Local de computador\\Meus** diretório.  
  
    2.  No computador em que você deseja configurar como um servidor de federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando.  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` é seu domínio do AD e o nome da sua conta gMSA nesse domínio. `<first_federation_server_hostname>` é o nome de host do servidor de Federação principal neste farm existente.  
  
        Você pode obter o valor para `<certificate_thumbprint>` executando `dir Cert:\LocalMachine\My` na etapa anterior.  
  
        > [!NOTE]  
        > Se isso não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um nó de fazenda de trabalho. Se você quiser criar um nó de farm de servidores de computadores que executam o SQL Server, você deve ter a instância do SQL Server já instalado e em operação.  
        >   
        > Você pode usar o seguinte comando para adicionar um servidor de Federação a um farm existente que esteja usando uma instância do SQL Server: `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` onde **SQL\_Host\_Name** é o nome do servidor no qual está executando o SQL Server, e **SQL\_instance\_name** é o nome da instância do SQL Server. Se você usar a instância padrão do SQL Server, use um **sessionState** valor de "**dados Source\ = < SQL\_Host\_Name >; integrado Security\ = True**".  
  
        > [!IMPORTANT]  
        > Se você quiser criar um farm AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012 e o SQL Server 2014.  
  
-   Se você deseja associar a um servidor de Federação a um farm usando uma conta de usuário de domínio existente, faça o seguinte:  
  
    1.  No computador em que você deseja configurar como um servidor de federação, abra a janela PowerShellcommand do Windows e, em seguida, execute o seguinte comando: `$fscred = get-credential`. Insira as credenciais de conta de usuário de domínio que você deseja usar para a conta de serviço de federação no formato domínio \ \ usuário nome.  
  
    2.  No computador em que você deseja configurar como um servidor de federação, certifique-se de que o certificado SSL necessário foi importado para o **repositório Local de computador\\Meus** diretório. Você pode verificar se o certificado SSL tiver sido importado, executando o comando a seguir na janela do Windows PowerShellcommand: `dir Cert:\LocalMachine\My`. O certificado é listado com sua impressão digital no **repositório Local de computador\\Meus** diretório.  
  
    3.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando.  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > Se isso não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um nó de fazenda de trabalho. Se você quiser criar um nó de farm de servidores de computadores que executam o SQL Server, você deve ter a instância do SQL Server já instalado e em operação. Você pode usar o seguinte comando para adicionar um servidor de Federação a um farm existente usando uma instância do SQL Server: `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` onde **SQL\_Host\_Name** é o nome do servidor no qual a instância do SQL Server é executado, e **SQL\_instance\_name** é o nome da instância do SQL Server. Se você usar a instância padrão do SQL Server, use um **sessionState** valor de "**dados Source\ = < SQL\_Host\_Name >; integrado Security\ = True**".  
  
        > [!IMPORTANT]  
        > Se você quiser criar um farm AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo SQL Server 2012 e o SQL Server 2014.  
  
## <a name="see-also"></a>Consulte também 

[AD FS implantação](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implantando um Farm de servidores de Federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

