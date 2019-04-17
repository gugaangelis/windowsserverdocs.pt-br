---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: Criar um aplicativo web usando OpenID se conectar com o AD FS 2016
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f8040b19576ac9de4ced43e6313cad69276a3d27
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016"></a>Criar um aplicativo web usando OpenID se conectar com o AD FS 2016

>Aplica-se a: Windows Server 2016

AD FS 2016 com base no suporte Oauth inicial no AD FS no Windows Server 2012 R2, introduz o suporte para usar o OpenId conectar logon.  
  
## <a name="pre-requisites"></a>Pré-requisitos  
A seguir está uma lista dos pré-requisitos necessários antes de concluir este documento. Este documento presume que o AD FS tiver sido instalado e um farm AD FS foi criado.  
  
-   Uma assinatura do Azure AD (uma avaliação gratuita serve)  
  
-   Ferramentas de cliente do GitHub  
  
-   AD FS no Windows Server 2016 TP4 ou posterior  
  
-   Visual Studio 2013 ou posterior.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Criar um grupo de aplicativos no AD FS 2016  
A seção a seguir descreve como configurar o grupo de aplicativos no AD FS 2016.  
  
#### <a name="create-application-group"></a>Criar um grupo de aplicativos  
  
1.  No AD FS gerenciamento, clique com botão direito em grupos de aplicativos e selecione **adicionar grupo de aplicativos**.  
  
2.  No Assistente de grupo do aplicativo, para o nome, digite **ADFSSSO** e, em **aplicativos autônomos**selecionar o **site ou aplicativo de servidor** modelo.  Clique em **próxima**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  
  
3.  Copiar o **identificador de cliente** valor.  Ele será usado mais tarde como o valor de ida: ClientId no arquivo de Web. config do aplicativo.  
  
4.  Digite o seguinte para **URI de redirecionamento:** - **https://localhost:44320/**.  Clique em **adicionar**. Clique em **próxima**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  
  
5.  No **configurar as credenciais do aplicativo** tela, coloque um check-in **gerar um segredo compartilhado** e copie o segredo. Clique em **próximo**  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)  
  
6.  Sobre o **resumo** de tela, clique em **próxima**.  
  
7.  Sobre o **Complete** de tela, clique em **fechar**.  
  
8.  Agora, em novo grupo de aplicativos com o botão direito e selecione **propriedades**.  
  
9. Sobre o **ADFSSSO propriedades** clique **Adicionar aplicativo**.  
  
10. Sobre o **adicionar um novo aplicativo para aplicativo de exemplo** selecione **API da Web** e clique em **próxima**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_4.PNG)  
  
11. Sobre o **configurar API da Web** tela, digite o seguinte para **identificador** - **https://contoso.com/WebApp**.  Clique em **adicionar**. Clique em **próxima**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
12. Sobre o **escolher política de controle de acesso** e selecione **permitir todos** e clique em **próxima**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. No **configurar permissões do aplicativo** tela, certifique-se de **openid** está marcada e clique em **próxima**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
14. Sobre o **resumo** de tela, clique em **próxima**.  
  
15. Sobre o **Complete** de tela, clique em **fechar**.  
  
16. Sobre o **propriedades do aplicativo de amostra** clique **Okey**.  
  
## <a name="download-and-modify-mvp-app-to-authenticate-via-openid-connect-and-ad-fs"></a>Baixe e modificar o aplicativo MVP para autenticar via OpenId conectar e AD FS  
Esta seção discute como baixar a amostra de API da Web e modificá-lo no Visual Studio.   Usaremos o exemplo do Azure AD [aqui](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  
  
Para baixar o projeto de exemplo, use Git Bash e digite o seguinte:  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  
  
#### <a name="to-modify-the-app"></a>Para modificar o aplicativo  
  
1.  Abra a amostra usando o Visual Studio.  
  
2.  Compile o aplicativo para que todos os NuGets ausentes são restaurados.  
  
3.  Abra o arquivo Web. config.  Modificar os seguintes valores para a aparência a seguir:  
  
    ```  
    <add key="ida:ClientId" value="8219ab4a-df10-4fbd-b95a-8b53c1d8669e" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://adfs.contoso.com/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:ResourceID" value="https://contoso.com/WebApp"  
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="https://localhost:44320/" />  
    ```  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  
  
4.  Abra o arquivo Startup.Auth.cs e faça as seguintes alterações:  
  
    -   Comente o seguinte:  
  
        ```  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
    -   Ajuste a lógica de inicialização de middleware OpenId conectar-se com as seguintes alterações:  
  
        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  
  
        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  
  
    -   Para mais longe para baixo, modificar as opções de middleware OpenId se conectar como o seguinte:  
  
        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                MetadataAddress = metadataAddress,  
                RedirectUri = postLogoutRedirectUri,  
                PostLogoutRedirectUri = postLogoutRedirectUri 
        ```  
  
        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_11.PNG)  
  
        Alterando acima estamos fazendo o seguinte:  
  
        -   Em vez de usar a autoridade para comunicar dados sobre o emissor confiável, podemos especificar a localização de documento de descoberta diretamente por meio de MetadataAddress  
  
        -   Azure AD não impõe a presença de uma redirect_uri na solicitação, mas o AD FS. Portanto, precisamos adicioná-lo aqui  
  
## <a name="verify-the-app-is-working"></a>Verifique se que o aplicativo está funcionando  
Depois que as alterações acima foram feitas, pressione F5.  Isso abrirá a página de exemplo.  Clique em login.  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  
  
Você será redirecionado para a página de entrada do AD FS.  Vá em frente e logon.  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  
  
Depois que isso for bem-sucedida, você deve ver o que você agora está conectado.  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  
  
## <a name="next-steps"></a>Próximas etapas
[AD FS desenvolvimento](../../ad-fs/AD-FS-Development.md)  

