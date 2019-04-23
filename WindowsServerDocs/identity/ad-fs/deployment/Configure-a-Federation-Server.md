---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: Guia de Implantação do AD FS do Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13ce514dc5f3f70217a26c898cde6fe24d4967c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847377"
---
# <a name="configure-a-federation-server"></a>Configurar um servidor de federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Depois de instalar os serviços de Federação do Active Directory \(do AD FS\) serviço de função em seu computador, você estará pronto para configurar este computador para se tornar um servidor de Federação. Você pode fazer o seguinte:  
  
-   [Configurar o primeiro servidor de Federação em um novo farm de servidores de Federação](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [Adicionar um servidor de Federação a um farm de servidor de Federação existente](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>Configurar o primeiro servidor de Federação em um novo farm de servidores de Federação  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Para configurar o primeiro servidor de Federação em um novo farm de servidores de Federação usando o Assistente de configuração do serviço de Federação Active Directory  
  
> [!NOTE]  
> Certifique-se de que você tenha permissões de administrador de domínio ou ter credenciais de administrador de domínio disponíveis antes de executar este procedimento.  
  
1.  Na página **Painel** do Gerenciador do Servidor, clique no sinalizador **Notificações** e, depois, clique em **Configurar o serviço de federação neste servidor**.  
  
    O **Assistente de Configuração do Serviço de Federação do Active Directory** é aberto.  
  
2.  Na página **Bem-vindo**, selecione **Criar o primeiro servidor de federação em um farm de servidores de federação** e clique em **Avançar**.  
  
3.  Sobre o **conectar ao AD DS** , especifique uma conta usando permissões de administrador de domínio para o Active Directory \(AD\) ao qual este computador está ingressado no domínio e clique **Avançar**.  
  
4.  Na página **Especificar Propriedades de Serviço** , siga estas etapas e clique em **Avançar**:  
  
    -   Importar o arquivo. pfx que contém o Secure Socket Layer \(SSL\) certificado e a chave que você obteve anteriormente. No [etapa 2: Registrar um certificado SSL para o AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), você obteve esse certificado e copiado para o computador que você deseja configurar como um servidor de Federação. Para importar o arquivo. pfx por meio do assistente, clique em **importação**e, em seguida, navegue até o local do arquivo. Insira a senha para o arquivo. pfx, quando for solicitado.  
  
    -   Forneça um nome para seu serviço de Federação. Por exemplo, **fs.contoso.com**. Esse nome deve corresponder a um dos nomes alternativos da entidade no certificado ou da entidade.  
  
    -   Forneça um nome de exibição para seu serviço de Federação. Por exemplo, **Contoso Corporation**. Os usuários visualizam esse nome em serviços de Federação do Active Directory \(do AD FS\) sinal\-na página.  
  
5.  Sobre o **especificar conta de serviço** , especifique uma conta de serviço. Você pode criar ou usar um conta de serviço gerenciado de grupo existente \(gMSA\) ou usar uma conta de usuário de domínio existente. Se você selecionar a opção de criar uma nova conta gMSA, especifique um nome para a nova conta. Se você selecionar a opção de usar uma gMSA existente ou uma conta de domínio, clique em **selecionar** para selecionar uma conta.  
  
    > [!NOTE]  
    > A vantagem de usar uma conta gMSA é seu automaticamente\-negociado o recurso de atualização de senha.  
  
    > [!WARNING]  
    > Se você quiser usar uma conta gMSA, você deve ter pelo menos um controlador de domínio em seu ambiente que está executando o sistema operacional Windows Server 2012.  
    >   
    > Se a opção gMSA estiver desabilitada, e você verá uma mensagem de erro, como **contas de serviço gerenciado do grupo não estão disponíveis porque a chave de raiz do KDS não foi definida**, você pode habilitar a gMSA no seu domínio executando o Windows a seguir Comando do PowerShell em um controlador de domínio que executa o Windows Server 2012 ou posterior, no domínio do Active Directory: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Volte para o assistente, clique em **Previous**e, em seguida, clique em **próxima** para re\-insira o **especificar conta de serviço** página. A opção gMSA agora deverá estar habilitada. Você pode selecioná-la e digite um nome de conta gMSA que você deseja usar.  
  
6.  Sobre o **especificar banco de dados de configuração** página, especifique um banco de dados de configuração do AD FS e, em seguida, clique em **próxima**. Você pode criar um banco de dados neste computador usando o banco de dados interno do Windows \(WID\), ou você pode especificar o local e o nome da instância do Microsoft SQL Server.  
  
    Para saber mais, confira [A função do banco de dados de configuração de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
    > [!IMPORTANT]  
    > Se você quiser criar um farm do AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo o SQL Server 2012 e SQL Server 2014.  
  
7.  Na página **Examinar Opções** , verifique suas seleções de configuração e clique em **Avançar**.  
  
8.  Sobre o **Pre\-verificações necessárias** página, verifique se todas as verificações de pré-requisito forem concluídas com êxito e, em seguida, clique em **configurar**.  
  
9. Sobre o **resultados** página, examine os resultados, verifique se a configuração for concluída com êxito e, em seguida, clique em **próximas etapas necessárias para concluir a implantação do serviço de Federação**. Para obter mais informações, consulte [próximas etapas para concluir a instalação do AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Clique em **Fechar** para sair do assistente.  
  
### <a name="BKMK_3"></a>Para configurar o primeiro servidor de Federação em um novo farm de servidores de federação por meio do Windows PowerShell  
Você pode criar um novo farm de servidores de Federação usando uma conta gMSA nova ou existente ou uma conta de usuário de domínio existente.  
  
-   **Se você quiser criar um novo servidor de Federação usando uma nova conta gMSA, faça o seguinte:**  
  
    > [!IMPORTANT]  
    > Você deve ter permissões de administrador de domínio para criar o primeiro servidor de Federação em um novo farm de servidores de Federação.  
  
    1.  No computador que você deseja configurar como um servidor de federação, certifique-se de que o certificado SSL necessário foi importado para o **computador Local\\Store My** directory. Você pode verificar se o certificado SSL foi importado executando o seguinte comando na janela de comando do Windows PowerShell: `dir Cert:\LocalMachine\My`. O certificado estiver listado por sua impressão digital na **computador Local\\Store My** directory.  
  
    2.  No controlador de domínio, abra a janela de comando do Windows PowerShell e execute o seguinte comando para verificar se a chave de raiz do KDS foi criada em seu domínio: `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Se não tiver sido criado para que a saída não exibe nenhuma informação, execute o seguinte comando para criar a chave: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.  
  
    3.  No computador que você deseja configurar como um servidor de federação, abra a janela de comando do Windows PowerShell e execute o seguinte comando:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > O `$` sinal no final do comando anterior é necessária.  
  
        Para obter o valor de `<certificate_thumbprint>`, execute `dir Cert:\LocalMachine\My`e, em seguida, selecione a impressão digital do seu certificado SSL. O valor de `<federation_service_name>` é o nome do serviço de federação, por exemplo, **fs.contoso.com**.  
  
        > [!NOTE]  
        > Se isso não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um farm de WID. Se você quiser criar um farm de servidores do SQL Server, você deve ter uma instância do SQL Server instalado e operacional.  
        >   
        > Você pode usar o comando a seguir para criar o primeiro servidor de Federação em um novo farm que usa uma instância do SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"` onde **< SQL\_Host\_nome >** é o nome do servidor no qual o SQL Servidor está em execução, e **< SQL\_instância\_nome >** é o nome da instância do SQL Server. Se você usar a instância padrão do SQL Server, use uma **SQLConnectionString** valor de "**fonte de dados\=< SQL\_Host\_nome >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Se você deseja criar um farm do AD FS e usar o SQL Server para armazenar seus dados de configuração, pode usar o SQL Server 2008 e versões mais recentes, incluindo o SQL Server 2012.  
  
-   **Se você quiser criar um novo servidor de Federação usando uma conta de usuário de domínio existente, faça o seguinte:**  
  
    1.  No computador que você deseja configurar como um servidor de federação, certifique-se de que o certificado SSL necessário foi importado para o **computador Local\\Store My** directory. Você pode verificar se o certificado SSL foi importado executando o seguinte comando na janela de comando do Windows PowerShell: `dir Cert:\LocalMachine\My`. O certificado estiver listado por sua impressão digital na **computador Local\\Store My** directory.  
  
    2.  No computador que você deseja configurar como um servidor de federação, abra a janela de comando do Windows PowerShell e, em seguida, execute o seguinte comando: `$fscred = Get-Credential`. Insira as credenciais de conta de usuário do domínio que você deseja usar para a conta de serviço de federação no domínio do formato\\nome de usuário.  
  
    3.  Na mesma janela de comando do Windows PowerShell, execute o seguinte comando:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        Para obter o valor de **< certificado\_impressão digital >**, execute `dir Cert:\LocalMachine\My`e, em seguida, selecione a impressão digital do seu certificado SSL. O valor de **< federação\_service\_nome >** é o nome do serviço de federação, por exemplo, fs.contoso.com.  
  
        > [!NOTE]  
        > Se isso não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um farm de WID. Se você quiser criar um farm do SQL Server, você deve ter a instância do SQL Server instalado e operacional.  
        >   
        > Você pode usar o comando a seguir para criar o primeiro servidor de Federação em um novo farm que usa uma instância do SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` onde **SQL\_Host\_nome** é o nome do servidor no qual o SQL Server é em execução, e **SQL\_instância\_nome** é o nome da instância do SQL Server. Se você usar a instância padrão do SQL Server, use uma **SQLConnectionString** valor de "**fonte de dados\=< SQL\_Host\_nome >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Se você quiser criar um farm do AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo o SQL Server 2012 e SQL Server 2014.  
  
## <a name="BKMK_2"></a>Adicionar um servidor de Federação a um farm de servidor de Federação existente  
  
> [!IMPORTANT]  
> Certifique-se de que você tenha concluído [etapa 3: Instalar o serviço de função do AD FS](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md), antes de iniciar qualquer um dos procedimentos nesta seção.  
  
> [!IMPORTANT]  
> Certifique-se de que você obteve um servidor SSL válido certificado de autenticação antes de concluir este procedimento.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Para adicionar um servidor de Federação a um farm de servidores de Federação existente via o Assistente de configuração do serviço de Federação Active Directory  
  
1.  Na página **Painel** do Gerenciador do Servidor, clique no sinalizador **Notificações** e, depois, clique em **Configurar o serviço de federação neste servidor**.  
  
    O **Assistente de Configuração do Serviço de Federação do Active Directory** é aberto.  
  
2.  Sobre o **bem-vindo** página, selecione **adicionar um servidor de Federação a um farm de servidores de Federação**e, em seguida, clique em **próxima**.  
  
3.  Sobre o **conectar ao AD DS** , especifique uma conta usando permissões de administrador de domínio para o domínio do AD para o qual este computador está associado e clique **próxima**.  
  
4.  Sobre o **especificar Farm** página, forneça o nome do servidor de Federação primário em um farm que usa o WID ou especifique o nome de host do banco de dados e o nome da instância de banco de dados de um farm de servidores de Federação existente que usa o SQL Server.  
  
    > [!WARNING]  
    > No Windows Server® 2012 R2, há uma solução alternativa para especificar a instância padrão do SQL Server. A solução alternativa é não usar a interface do usuário. Em vez disso, use as etapas em [para configurar o primeiro servidor de Federação em um novo farm de servidores de federação por meio do Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).  
  
    > [!IMPORTANT]  
    > Se você deseja criar um farm do AD FS e usar o SQL Server para armazenar seus dados de configuração, pode usar o SQL Server 2008 e versões mais recentes, incluindo o SQL Server 2012.  
  
5.  Sobre o **especificar o certificado SSL** página, importe o arquivo. pfx que contém o certificado SSL e a chave que você obteve anteriormente. Esse é o certificado obrigatório de autenticação de serviço. No [etapa 2: Registrar um certificado SSL para o AD FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), você obteve esse certificado e copiou para o computador que você deseja configurar como um servidor de Federação. Para importar o arquivo. pfx por meio do assistente, clique em **importação** e navegue até o local do arquivo. Insira a senha para o arquivo. pfx, quando for solicitado.  
  
6.  Sobre o **especificar conta de serviço** , especifique a mesma conta de serviço que você configurou quando criou o primeiro servidor de federação no farm. Você pode usar um conta de serviço gerenciado de grupo existente ou uma conta de usuário de domínio existente.  
  
    > [!IMPORTANT]  
    > A conta que você especifica deve ser a mesma conta como a conta que foi usada no servidor de Federação principal neste farm.  
  
7.  Na página **Examinar Opções** , verifique suas seleções de configuração e clique em **Avançar**.  
  
8.  Sobre o **Pre\-verificações necessárias** página, verifique se todas as verificações de pré-requisito forem concluídas com êxito e, em seguida, clique em **configurar**.  
  
9. Sobre o **resultados** página, examine os resultados, verifique se a configuração for concluída com êxito e, em seguida, clique em **próximas etapas necessárias para concluir a implantação do serviço de Federação**. Para obter mais informações, consulte [próximas etapas para concluir a instalação do AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Clique em **Fechar** para sair do assistente.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Para adicionar um servidor de Federação a um farm de servidor de Federação existente por meio do Windows PowerShell  
Você pode adicionar um servidor de Federação a um farm existente usando uma conta gMSA existente ou uma conta de usuário de domínio existente.  
  
-   Se você quiser associar um servidor de Federação a um farm usando uma conta Dmsa existente, faça o seguinte:  
  
    1.  No computador que você deseja configurar como um servidor de federação, certifique-se de que o certificado SSL necessário foi importado para o **computador Local\\Store My** directory. Você pode verificar se o certificado SSL foi importado executando o seguinte comando na janela de comando do Windows PowerShell: `dir Cert:\LocalMachine\My`. O certificado estiver listado por sua impressão digital na **computador Local\\Store My** directory.  
  
    2.  No computador que você deseja configurar como um servidor de federação, abra a janela de comando do Windows PowerShell e execute o comando a seguir.  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` é o seu domínio do AD e o nome da sua conta gMSA naquele domínio. `<first_federation_server_hostname>` é o nome do host do servidor de Federação principal neste farm existente.  
  
        Você pode obter o valor de `<certificate_thumbprint>` executando `dir Cert:\LocalMachine\My` na etapa anterior.  
  
        > [!NOTE]  
        > Se isso não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um nó do farm WID. Se você quiser criar um nó de farm de servidor dos computadores que executam o SQL Server, você deve ter a instância do SQL Server instalado e operacional.  
        >   
        > Você pode usar o comando a seguir para adicionar um servidor de Federação a um farm existente que está usando uma instância do SQL Server: `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` onde **SQL\_Host\_nome** é o nome do servidor no qual o SQL Server é em execução, e **SQL\_instância\_nome** é o nome da instância do SQL Server. Se você usar a instância padrão do SQL Server, use uma **SQLConnectionString** valor de "**fonte de dados\=< SQL\_Host\_nome >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Se você quiser criar um farm do AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo o SQL Server 2012 e SQL Server 2014.  
  
-   Se você quiser associar um servidor de Federação a um farm usando uma conta de usuário de domínio existente, faça o seguinte:  
  
    1.  No computador que você deseja configurar como um servidor de federação, abra a janela de Windows PowerShellcommand e, em seguida, execute o seguinte comando: `$fscred = get-credential`. Insira as credenciais de conta de usuário do domínio que você deseja usar para a conta de serviço de federação no domínio do formato\\nome de usuário.  
  
    2.  No computador que você deseja configurar como um servidor de federação, certifique-se de que o certificado SSL necessário foi importado para o **computador Local\\Store My** directory. Você pode verificar se o certificado SSL foi importado executando o seguinte comando na janela do Windows PowerShellcommand: `dir Cert:\LocalMachine\My`. O certificado estiver listado por sua impressão digital na **computador Local\\Store My** directory.  
  
    3.  Na mesma janela de comando do Windows PowerShell, execute o comando a seguir.  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > Se isso não for a primeira vez que você executar esse comando, adicione o `OverwriteConfiguration` parâmetro.  
  
        > [!NOTE]  
        > O comando anterior cria um nó do farm WID. Se você quiser criar um nó de farm de servidor dos computadores que executam o SQL Server, você deve ter a instância do SQL Server instalado e operacional. Você pode usar o comando a seguir para adicionar um servidor de Federação a um farm existente, usando uma instância do SQL Server: `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` onde **SQL\_Host\_nome** é o nome do servidor no qual a instância do SQL Servidor está em execução, e **SQL\_instância\_nome** é o nome da instância do SQL Server. Se você usar a instância padrão do SQL Server, use uma **SQLConnectionString** valor de "**fonte de dados\=< SQL\_Host\_nome >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Se você quiser criar um farm do AD FS e usar o SQL Server para armazenar os dados de configuração, você pode usar o SQL Server 2008 e versões mais recentes, incluindo o SQL Server 2012 e SQL Server 2014.  
  
## <a name="see-also"></a>Consulte também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implantar um Farm de servidores de Federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

