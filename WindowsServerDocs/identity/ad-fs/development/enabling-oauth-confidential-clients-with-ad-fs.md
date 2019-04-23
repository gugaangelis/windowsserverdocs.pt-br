---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: Criar um aplicativo do lado do servidor usando clientes confidenciais OAuth com o AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 175c683f9097aeba4c1f06e8671183476c98aa3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869577"
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016"></a>Criar um aplicativo do lado do servidor usando clientes confidenciais OAuth com o AD FS 2016

>Aplica-se a: Windows Server 2016

Aproveitando o suporte Oauth inicial no AD FS no Windows Server 2012 R2, o AD FS 2016 introduz suporte para clientes que mantêm seu próprios segredo, como um aplicativo ou serviço em execução em um servidor web.  Esses clientes são conhecidos como clientes confidenciais.    
Abaixo é um esquema de um aplicativo web em execução em um servidor web e que serve como um cliente confidencial para o AD FS:  
  
## <a name="pre-requisites"></a>Pré-requisitos  
A seguir é uma lista de pré-requisitos são necessários antes de concluir este documento. Este documento pressupõe que o AD FS foi instalado e um farm do AD FS tiver sido criado.  
  
-   Uma assinatura do Azure AD (uma avaliação gratuita é bom)  
  
-   Ferramentas de cliente do GitHub  
  
-   O AD FS no Windows Server 2016 TP4 ou posterior  
  
-   Visual Studio 2013 ou posterior.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Criar um grupo de aplicativos no AD FS 2016  
A seção a seguir descreve como configurar o grupo de aplicativos do AD FS 2016.  
  
#### <a name="create-the-application-group"></a>Criar o grupo de aplicativos  
  
1.  No gerenciamento do AD FS, clique duas vezes em grupos de aplicativos e selecione **adicionar grupo de aplicativos**.  
  
2.  No Assistente de grupo do aplicativo, para o nome, digite **ADFSOAUTHCC** e, em **aplicativos autônomos** selecione o **aplicativo de servidor ou site** modelo.  Clique em **Avançar**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  
  
3.  Cópia de **identificador de cliente** valor.  Ele será usado posteriormente como o valor para **ida: ClientId** no arquivo de Web. config do aplicativo.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  
  
4.  Insira o seguinte para **URI de redirecionamento:** - **https://localhost:44323**.  Clique em **Adicionar**. Clique em **Avançar**.  
  
5.  Sobre o **configurar credenciais de aplicativo** tela, colocar uma marca de seleção **gerar um segredo compartilhado**e copie o segredo.  Isso será usado posteriormente como o valor para **ida: AppKey** no arquivo de Web. config do aplicativo.  Clique em **Avançar**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)  
  
6.  Sobre o **resumo** tela, clique em **próxima**.  
  
7.  Sobre o **Complete** tela, clique em **fechar**.  
  
8.  Agora, no novo grupo de aplicativos botão direito do mouse e selecione **propriedades**.  
  
9. Na **propriedades ADFSOAUTHCC** clique em **Adicionar aplicativo**.  
  
10. Sobre o **adicionar um novo aplicativo ao aplicativo de exemplo** selecionar **API da Web**e clique em **próxima**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)  
  
11. Sobre o **configurar a API da Web** tela, insira o seguinte para **identificador** - **https://contoso.com/WebApp**.  Clique em **Adicionar**. Clique em **Avançar**.  Esse valor será usado posteriormente para **ida: GraphResourceId** no arquivo de Web. config do aplicativo.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  
  
12. Sobre o **escolher política de controle de acesso** tela, selecione **permitir todos** e clique em **próxima**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. No **configurar permissões do aplicativo** tela, verifique se**user_impersonation** está selecionado e clique em **próxima**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  
  
14. Sobre o **resumo** tela, clique em **próxima**.  
  
15. Sobre o **Complete** tela, clique em **fechar**.  
  
16. Sobre o **propriedades ADFSOAUTHCC** clique em **Okey**.  
  
## <a name="upgrade-the-database"></a>Atualizar o banco de dados  
Visual Studio 2015 foi usado na criação deste passo a passo.   Para obter o exemplo a trabalhar com o Visual Studio 2015, você precisará atualizar o arquivo de banco de dados.  Use o procedimento a seguir para fazer isso.  
  
Esta seção discute como baixar o exemplo de API da Web e atualizar o banco de dados no Visual Studio 2015.   Vamos usar o exemplo do Azure AD que está [aqui](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity).  
  
Para baixar o projeto de exemplo, use Git Bash e digite o seguinte:  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  
  
![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  
  
#### <a name="to-upgrade-the-database-file"></a>Para atualizar o arquivo de banco de dados  
  
1.  Abra o projeto no Visual Studio, haverá um pop-up informando que o aplicativo requer o SQL Server 2102 Express ou você precisará atualizar o banco de dados.  Clique em Okey.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  
  
2.  Próxima compilação o aplicativo selecionando Build -> Compilar solução na parte superior.  Isso irá restaurar todos os pacotes do NuGet.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  
  
3.  Agora, na parte superior, selecione **modo de exibição** -> **Gerenciador de servidores**.  Depois de aberto, sob **conexões de dados**, clique com botão direito **DefaultConnection** e selecione **Modificar Conexão**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  
  
4.  Na **Modificar Conexão**, selecione **avançado**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  
  
5.  Em Propriedades avançadas, localize a fonte de dados e use a lista suspensa alterá-lo no **(LocalDb\v11.0)** à **MSSQLLocalDB (LoaclDB)**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  
  
6.  Clique em Okey. Clique em Okey.  Clique em Sim para atualizar o banco de dados.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  
  
7.  Quando isso for concluído, mais à direita, copie o valor na caixa ao lado **cadeia de caracteres de Conexão.**  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  
  
8.  Agora, abra o arquivo Web. config e substitua o valor connectionString com o valor que você copiou acima.  Salve o arquivo Web. config.  
  
    > [!NOTE]  
    > As etapas acima são necessárias para que podemos obter a connectionString novo.  Caso contrário, quando executamos o Update-Database abaixo ocorrerá um erro.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  
  
9. Na parte superior do Visual Studio, selecione **modo de exibição** -> **Other Windows** -> **Package Manager Console**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  
  
10. Na parte inferior, no Console do Gerenciador de pacotes, digite: `Enable-Migrations` e pressione enter.  
  
    > [!NOTE]  
    > Se você receber um erro que diz Enable-Migrations não é reconhecido como um cmdlet, digite Install-Package EntityFramework para atualizar o EntityFramework.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  
  
11. Na parte inferior, no Console do Gerenciador de pacotes, digite: `Add-Migration <anynamehere>` e pressione enter.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  
  
12. Na parte inferior, no Console do Gerenciador de pacotes, digite: `Update-Database` e pressione enter.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  
  
## <a name="modify-the-webapi-in-visual-studio"></a>Modificar a API da Web no Visual Studio  
  
#### <a name="to-modify-the-sample-web-api"></a>Para modificar a API da Web de exemplo  
  
1.  Abra o exemplo usando o Visual Studio.  
  
2.  Abra o arquivo Web. config.  Modifique os seguintes valores:  
  
    -   ida: ClientId - insira o valor do #3 acima.  
  
    -   ida: AppKey - insira o valor do #5 acima.  
  
    -   ida: GraphResourceId - insira o valor de #11 acima.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  
  
3.  Abra o arquivo Startup.Auth.cs em App_Start e faça as seguintes alterações:  
  
    -   Comente as linhas a seguir:  
  
        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
        ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  
  
    -   Adicione o seguinte em seu lugar:  
  
        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  
  
        onde < your_fsname > é substituído com a parte de sua url de serviço de federação, por exemplo adfs.contoso.com DNS  
  
        ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_26.PNG)  
  
4.  Abra o arquivo UserProfileController.cs e faça as seguintes alterações:  
  
    -   Comente o seguinte:  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  
  
    -   Substitua as duas ocorrências com o seguinte:  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  
  
        ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  
  
    -   Comente o seguinte:  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  
  
    -   Substitua as duas ocorrências com o seguinte:  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  
  
        ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  
  
    -   Comente agora todas as instâncias das seguintes opções:  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString() + "/OAuth");  
        ```  
  
    -   Substitua todas as ocorrências pelo seguinte:  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString());  
        ```  
  
        ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  
  
## <a name="test-the-solution"></a>Testar a solução  
Nesta seção, testaremos a solução de cliente confidencial.  Use o procedimento a seguir para testar a solução.  
  
#### <a name="testing-the-confidential-client-solution"></a>Testando a solução de cliente confidencial  
  
1.  Na parte superior do Visual Studio, verifique se o que Internet Explorer está selecionado e clique na seta verde.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  
  
2.  Depois que a página ASP.Net é exibida, clique em **registrar**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
3.  Insira um nome de usuário e senha e, em seguida, clique em **registrar**.  Isso cria uma conta local no banco de dados SQL.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
4.  Observe que agora, o site do ASP.NET diz Hello bsimon!.  Clique em **perfil**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  
  
5.  Isso abre uma página sem qualquer informação e diz que podemos deve clique aqui para entrar.  Clique em **aqui**.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  
  
6.  Agora você será solicitado a entrar no AD FS.  
  
    ![Oauth do AD FS](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  
  
## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  

