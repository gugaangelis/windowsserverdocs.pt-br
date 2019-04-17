---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: Criar um aplicativo de lado do servidor usando OAuth confidenciais clientes com o AD FS 2016
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 175c683f9097aeba4c1f06e8671183476c98aa3f
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016"></a>Criar um aplicativo de lado do servidor usando OAuth confidenciais clientes com o AD FS 2016

>Aplica-se a: Windows Server 2016

Com base no suporte Oauth inicial no AD FS no Windows Server 2012 R2, o AD FS 2016 apresenta suporte para clientes capazes de manutenção de seu próprios segredo, como um aplicativo ou serviço em execução em um servidor web.  Esses clientes são conhecidos como clientes confidenciais.    
Abaixo está um esquema de um aplicativo web em execução em um servidor web e servindo como um cliente confidencial do AD FS:  
  
## <a name="pre-requisites"></a>Pré-requisitos  
A seguir está uma lista dos pré-requisitos necessários antes de concluir este documento. Este documento presume que o AD FS tiver sido instalado e um farm AD FS foi criado.  
  
-   Uma assinatura do Azure AD (uma avaliação gratuita serve)  
  
-   Ferramentas de cliente do GitHub  
  
-   AD FS no Windows Server 2016 TP4 ou posterior  
  
-   Visual Studio 2013 ou posterior.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Criar um grupo de aplicativos no AD FS 2016  
A seção a seguir descreve como configurar o grupo de aplicativos no AD FS 2016.  
  
#### <a name="create-the-application-group"></a>Criar o grupo de aplicativos  
  
1.  No AD FS gerenciamento, clique com botão direito em grupos de aplicativos e selecione **adicionar grupo de aplicativos**.  
  
2.  No Assistente de grupo do aplicativo, para o nome, digite **ADFSOAUTHCC** e, em **aplicativos autônomos** selecionar o **site ou aplicativo de servidor** modelo.  Clique em **próxima**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  
  
3.  Copiar o **identificador de cliente** valor.  Ele será usado mais tarde como o valor de **ida: ClientId** no arquivo de Web. config do aplicativo.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  
  
4.  Digite o seguinte para **URI de redirecionamento:** - **https://localhost:44323**.  Clique em **adicionar**. Clique em **próxima**.  
  
5.  No **configurar as credenciais do aplicativo** tela, coloque um check-in **gerar um segredo compartilhado**e copie o segredo.  Isso será usado mais tarde como o valor de **ida: AppKey** no arquivo de Web. config do aplicativo.  Clique em **próxima**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)  
  
6.  Sobre o **resumo** de tela, clique em **próxima**.  
  
7.  Sobre o **Complete** de tela, clique em **fechar**.  
  
8.  Agora, em novo grupo de aplicativos com o botão direito e selecione **propriedades**.  
  
9. Em **ADFSOAUTHCC propriedades** clique **Adicionar aplicativo**.  
  
10. Sobre o **adicionar um novo aplicativo para aplicativo de exemplo** selecione **API da Web**e clique em **próxima**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)  
  
11. Sobre o **configurar API da Web** tela, digite o seguinte para **identificador** - **https://contoso.com/WebApp**.  Clique em **adicionar**. Clique em **próxima**.  Esse valor será usado posteriormente para **ida: GraphResourceId** no arquivo de Web. config do aplicativo.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  
  
12. Sobre o **escolher política de controle de acesso** e selecione **permitir todos** e clique em **próxima**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. No **configurar permissões do aplicativo** tela, certifique-se de**user_impersonation** está marcada e clique em **próxima**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  
  
14. Sobre o **resumo** de tela, clique em **próxima**.  
  
15. Sobre o **Complete** de tela, clique em **fechar**.  
  
16. Sobre o **ADFSOAUTHCC propriedades** clique **Okey**.  
  
## <a name="upgrade-the-database"></a>Atualizar o banco de dados  
Visual Studio 2015 foi usado na criação deste passo a passo.   Para obter o exemplo de trabalhar com o Visual Studio 2015, você precisará atualizar o arquivo de banco de dados.  Use o procedimento a seguir para fazer isso.  
  
Esta seção discute como baixar a amostra de API da Web e atualizar o banco de dados no Visual Studio 2015.   Usaremos o exemplo do Azure AD [aqui](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity).  
  
Para baixar o projeto de exemplo, use Git Bash e digite o seguinte:  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  
  
![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  
  
#### <a name="to-upgrade-the-database-file"></a>Para atualizar o arquivo de banco de dados  
  
1.  Abra o projeto no Visual Studio, haverá um pop-up informando que o aplicativo requer 2102 do SQL Server Express ou você precisará atualizar o banco de dados.  Clique em Okey.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  
  
2.  Próxima compilação o aplicativo selecionando Compilar -> Compilar solução na parte superior.  Isso irá restaurar todos os pacotes NuGet.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  
  
3.  Agora na parte superior, selecione **exibição** -> **Gerenciador de servidores**.  Depois que abrir, em **conexões de dados**, clique com botão direito **DefaultConnection** e selecione **Modificar Conexão**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  
  
4.  Em **Modificar Conexão**, selecione **avançado**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  
  
5.  Em Propriedades avançadas, localize a fonte de dados e usar na lista suspensa para alterá-lo de **(LocalDb\v11.0)** para **(LoaclDB) MSSQLLocalDB**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  
  
6.  Clique em Okey. Clique em Okey.  Clique em Sim para atualizar o banco de dados.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  
  
7.  Quando isso for concluído, mais à direita, copie o valor na caixa ao lado de **cadeia de caracteres de Conexão.**  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  
  
8.  Agora, abra o arquivo Web. config e substitua o valor que consta connectionString com o valor que você copiou acima.  Salve o arquivo Web. config.  
  
    > [!NOTE]  
    > As etapas acima são necessárias para que possamos obter novos connectionString.  Caso contrário, quando executamos Update-Database abaixo ela emite um erro.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  
  
9. Na parte superior do Visual Studio, selecione **exibição** -> **Other Windows** -> **Package Manager Console**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  
  
10. Na parte inferior, no Console do Gerenciador de pacotes, digite: `Enable-Migrations`e pressione enter.  
  
    > [!NOTE]  
    > Se você receber um erro que diz Enable-Migrations não é reconhecido como um cmdlet, digite EntityFramework Install-Package para atualizar o EntityFramework.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  
  
11. Na parte inferior, no Console do Gerenciador de pacotes, digite: `Add-Migration <anynamehere>`e pressione enter.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  
  
12. Na parte inferior, no Console do Gerenciador de pacotes, digite: `Update-Database`e pressione enter.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  
  
## <a name="modify-the-webapi-in-visual-studio"></a>Modificar o WebApi no Visual Studio  
  
#### <a name="to-modify-the-sample-web-api"></a>Para modificar a API da Web de exemplo  
  
1.  Abra a amostra usando o Visual Studio.  
  
2.  Abra o arquivo Web. config.  Modificar os seguintes valores:  
  
    -   ida: ClientId - insira o valor de #3 acima.  
  
    -   ida: AppKey - insira o valor de 5 # acima.  
  
    -   ida: GraphResourceId - insira o valor de 11 # acima.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  
  
3.  Abra o arquivo Startup.Auth.cs em App_Start e faça as seguintes alterações:  
  
    -   Comente as seguintes linhas:  
  
        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  
  
    -   Adicione o seguinte em seu lugar:  
  
        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  
  
        Onde < your_fsname > é substituído com a parte de sua url de serviço de federação, por exemplo adfs.contoso.com DNS  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_26.PNG)  
  
4.  Abra o arquivo UserProfileController.cs e faça as seguintes alterações:  
  
    -   Comente o seguinte:  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  
  
    -   Substitua ambas as ocorrências com o seguinte:  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  
  
    -   Comente o seguinte:  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  
  
    -   Substitua ambas as ocorrências com o seguinte:  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  
  
    -   Comente agora todas as instâncias das seguintes opções:  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString() + "/OAuth");  
        ```  
  
    -   Substitua todas as ocorrências com o seguinte:  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString());  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  
  
## <a name="test-the-solution"></a>Testar a solução  
Nesta seção, nós testará a solução de cliente confidenciais.  Use o procedimento a seguir para testar a solução.  
  
#### <a name="testing-the-confidential-client-solution"></a>Testando a solução de cliente confidenciais  
  
1.  Na parte superior do Visual Studio, verifique se o que Internet Explorer está selecionado e clique na seta verde.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  
  
2.  Depois que a página ASP.Net aparece, clique em **registrar**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
3.  Insira um nome de usuário e senha e clique em **registrar**.  Isso cria uma conta local no banco de dados SQL.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
4.  Aviso agora, o site ASP.NET diz bsimon Olá!.  Clique em **perfil**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  
  
5.  Isso abre uma página sem qualquer informação e diz que nós deve clicar aqui para entrar.  Clique em **aqui**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  
  
6.  Você agora precisará entrar no AD FS.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  
  
## <a name="next-steps"></a>Próximas etapas
[AD FS desenvolvimento](../../ad-fs/AD-FS-Development.md)  

