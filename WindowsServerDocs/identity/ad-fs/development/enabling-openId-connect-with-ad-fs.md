---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: Criar uma web do aplicativo usando o OpenID Connect com o AD FS 2016 e posterior
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbd42941f8952fc7f649636d2f3645f941240d49
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190420"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016-and-later"></a>Criar uma web do aplicativo usando o OpenID Connect com o AD FS 2016 e posterior

## <a name="pre-requisites"></a>Pré-requisitos  
A seguir é uma lista de pré-requisitos são necessários antes de concluir este documento. Este documento pressupõe que o AD FS foi instalado e um farm do AD FS tiver sido criado.  

-   Ferramentas de cliente do GitHub  

-   O AD FS no Windows Server 2016 TP4 ou posterior  

-   Visual Studio 2013 ou posterior.  

## <a name="create-an-application-group-in-ad-fs-2016-and-later"></a>Criar um grupo de aplicativos no AD FS 2016 e posterior
A seção a seguir descreve como configurar o aplicativo de grupo no AD FS 2016 e posterior.  

#### <a name="create-application-group"></a>Criar grupo de aplicativos  

1.  No gerenciamento do AD FS, clique duas vezes em grupos de aplicativos e selecione **adicionar grupo de aplicativos**.  

2.  No Assistente de grupo do aplicativo, para o nome, digite **ADFSSSO** e, em **aplicativos cliente-servidor** selecione o **navegador da Web acessando um aplicativo web** modelo.  Clique em **Avançar**.

    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  

3.  Cópia de **identificador de cliente** valor.  Ele será usado posteriormente como o valor para ida: ClientId no arquivo de Web. config do aplicativo.  

4.  Insira o seguinte para **URI de redirecionamento:**  -  **https://localhost:44320/** .  Clique em **Adicionar**. Clique em **Avançar**.  

    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  

5.  Sobre o **resumo** tela, clique em **próxima**.  

    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)

6.  Sobre o **Complete** tela, clique em **fechar**.  

## <a name="download-and-modify-sample-application-to-authenticate-via-openid-connect-and-ad-fs"></a>Baixe e modifique o aplicativo de exemplo para autenticar por meio do OpenID Connect e AD FS  
Esta seção discute como baixar o aplicativo Web de exemplo e modificá-lo no Visual Studio.   Vamos usar o exemplo do Azure AD que está [aqui](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Para baixar o projeto de exemplo, use Git Bash e digite o seguinte:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  

#### <a name="to-modify-the-app"></a>Para modificar o aplicativo  

1.  Abra o exemplo usando o Visual Studio.  

2.  Recompile o aplicativo para que todos os NuGets ausentes são restaurados.  

3.  Abra o arquivo Web. config.  Modifique os valores a seguir para que a aparência semelhante à seguinte:  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 in above section]" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with Redirect URI from #4 in the above section]" />  
    ```  

    ![OpenID do AD FS](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  

4.  Abra o arquivo Startup.Auth.cs e faça as seguintes alterações:  

    -   Comente o seguinte:  

        ```  
        //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
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

    -   Modificar ainda mais para baixo, as opções de middleware OpenId Connect da seguinte maneira:  

        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                MetadataAddress = metadataAddress,  
                PostLogoutRedirectUri = postLogoutRedirectUri,
                RedirectUri = postLogoutRedirectUri
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
