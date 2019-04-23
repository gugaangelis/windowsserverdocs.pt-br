---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: Criar um aplicativo web usando o OpenID Connect com o AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74a493e6568b71a05116140ec67586d36f439aa8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882657"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016"></a>Criar um aplicativo web usando o OpenID Connect com o AD FS 2016

>Aplica-se a: Windows Server 2016

Aproveitando o suporte Oauth inicial no AD FS no Windows Server 2012 R2, o AD FS 2016 introduz o suporte para usar o OpenId Connect logon.  
  
## <a name="pre-requisites"></a>Pré-requisitos  
A seguir é uma lista de pré-requisitos são necessários antes de concluir este documento. Este documento pressupõe que o AD FS foi instalado e um farm do AD FS tiver sido criado.  
  
-   Ferramentas de cliente do GitHub  
  
-   O AD FS no Windows Server 2016 TP4 ou posterior  
  
-   Visual Studio 2013 ou posterior.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Criar um grupo de aplicativos no AD FS 2016  
A seção a seguir descreve como configurar o grupo de aplicativos do AD FS 2016.  
  
#### <a name="create-application-group"></a>Criar grupo de aplicativos  
  
1.  No gerenciamento do AD FS, clique duas vezes em grupos de aplicativos e selecione **adicionar grupo de aplicativos**.  
  
2.  No Assistente de grupo do aplicativo, para o nome, digite **ADFSSSO** e, em **aplicativos autônomos** selecione o **aplicativo de servidor ou site** modelo.  Clique em **Avançar**.  
  
    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  
  
3.  Cópia de **identificador de cliente** valor.  Ele será usado posteriormente como o valor para ida: ClientId no arquivo de Web. config do aplicativo.  
  
4.  Insira o seguinte para **URI de redirecionamento:** - **https://localhost:44320/**.  Clique em **Adicionar**. Clique em **Avançar**.  
  
    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  
  
5.  Sobre o **configurar credenciais de aplicativo** tela, colocar uma marca de seleção **gerar um segredo compartilhado** e copie o segredo. Clique em **Avançar**.  
  
    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)  
  
6.  Sobre o **resumo** tela, clique em **próxima**.  
  
7.  Sobre o **Complete** tela, clique em **fechar**.  
  
8.  Agora, no novo grupo de aplicativos botão direito do mouse e selecione **propriedades**.  
  
9. Sobre o **propriedades ADFSSSO** clique em **Adicionar aplicativo**.  
  
10. Sobre o **adicionar um novo aplicativo ao aplicativo de exemplo** selecionar **API da Web** e clique em **próxima**.  
  
    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_4.PNG)  
  
11. Sobre o **configurar a API da Web** tela, insira o seguinte para **identificador** - **https://contoso.com/WebApp**.  Clique em **Adicionar**. Clique em **Avançar**.  
  
    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG) 
    
12. Sobre o **escolher política de controle de acesso** tela, selecione **permitir todos** e clique em **próxima**.  
  
    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. No **configurar permissões do aplicativo** tela, verifique se **openid** está selecionado e clique em **próxima**.  
  
    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
14. Sobre o **resumo** tela, clique em **próxima**.  
  
15. Sobre o **Complete** tela, clique em **fechar**.  
  
16. Sobre o **propriedades do aplicativo de exemplo** clique em **Okey**.  
  
## <a name="download-and-modify-mvp-app-to-authenticate-via-openid-connect-and-ad-fs"></a>Baixe e modifique o aplicativo MVP para autenticar por meio do OpenId Connect e AD FS  
Esta seção discute como baixar o exemplo de API da Web e modificá-lo no Visual Studio.   Vamos usar o exemplo do Azure AD que está [aqui](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  
  
Para baixar o projeto de exemplo, use Git Bash e digite o seguinte:  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  
  
![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  
  
#### <a name="to-modify-the-app"></a>Para modificar o aplicativo  
  
1.  Abra o exemplo usando o Visual Studio.  
  
2.  Compile o aplicativo para que todos os NuGets ausentes são restaurados.  
  
3.  Abra o arquivo Web. config.  Modifique os valores a seguir para que a aparência semelhante à seguinte:  
  
    ```  
    <add key="ida:ClientId" value="8219ab4a-df10-4fbd-b95a-8b53c1d8669e" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://adfs.contoso.com/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:ResourceID" value="https://contoso.com/WebApp"  
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="https://localhost:44320/" />  
    ```  
  
    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  
  
4.  Abra o arquivo Startup.Auth.cs e faça as seguintes alterações:  
  
    -   Comente o seguinte:  
  
        ```  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
    -   Ajuste a lógica de inicialização de middleware OpenId Connect com as seguintes alterações:  
  
        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  
  
        ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  
  
    -   Mais para baixo, modifique as opções de middleware OpenId Connect da seguinte maneira:  
  
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
  
        ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_11.PNG)  
  
        Alterando as opções acima estamos fazendo o seguinte:  
  
        -   Em vez de usar a autoridade para se comunicar dados sobre o emissor confiável, podemos especificar o local de documento de descoberta diretamente por meio de MetadataAddress  
  
        -   Azure AD não impõe a presença de um redirect_uri na solicitação, mas o ADFS faz. Portanto, precisamos adicioná-la aqui  
  
## <a name="verify-the-app-is-working"></a>Verifique se que o aplicativo está funcionando  
Depois que foram feitas as alterações acima, pressione F5.  Isso abrirá a página de exemplo.  Clique em entrar.  
  
![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  
  
Você será redirecionado para a página de entrada do AD FS.  Vá em frente e entre.  
  
![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  
  
Quando essa operação for bem-sucedida, você deve ver o que você agora está conectado.  
  
![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  
  
## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  

