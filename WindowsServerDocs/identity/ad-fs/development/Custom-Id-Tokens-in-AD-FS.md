---
title: Personalizar declarações a ser emitido no id_token ao usar o OAuth ou OpenID Connect com o AD FS 2016 ou posterior
description: Uma visão geral técnica dos conceitos de token de id personalizada no AD FS 2016 ou posterior
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 04573aa13689a0e6744b01a0fbf8b11b622b2706
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445475"
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>Personalizar declarações a ser emitido no id_token ao usar o OAuth ou OpenID Connect com o AD FS 2016 ou posterior

## <a name="overview"></a>Visão geral
O artigo [aqui](native-client-with-ad-fs.md) mostra como criar um aplicativo que usa o AD FS para o OpenID Connect do logon. No entanto, por padrão existem apenas um conjunto fixo de declarações disponíveis no id_token. AD FS 2016 e versões posteriores têm a capacidade de personalizar o id_token em cenários de OpenID Connect.

## <a name="when-are-custom-id-token-used"></a>Quando são ID personalizada token usado?
Em determinados cenários, é possível que o aplicativo cliente não tem um recurso que está tentando acessar. Portanto, ele não precisa realmente um token de acesso. Nesses casos, o aplicativo cliente precisa basicamente apenas um ID token, mas com algumas declarações adicionais para ajudá-lo a funcionalidade.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>Quais são as restrições sobre como obter declarações personalizadas na ID de token?

### <a name="scenario-1"></a>Cenário 1

![restringir](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode é definido como form_post
2.  Somente os clientes públicos podem obter declarações personalizadas na ID de token
3.  Identificador da terceira parte (identificador de API da Web) deve ser o mesmo que o identificador de cliente

### <a name="scenario-2"></a>Cenário 2

![restringir](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Com o [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) instalado nos servidores do AD FS
1.  response_mode é definido como form_post
2.  Clientes públicos e confidenciais podem obter declarações personalizadas na ID de token
3.  Atribua allatclaims de escopo para o cliente – par da RP.
Você pode atribuir o escopo usando o cmdlet Grant ADFSApplicationPermission conforme indicado no exemplo a seguir:

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Criando e configurando um aplicativo OAuth para lidar com declarações personalizadas no token de ID
Siga as etapas abaixo para criar e configurar o aplicativo no AD FS para receber o token de ID com declarações personalizadas.

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>Criar e configurar um grupo de aplicativos no AD FS 2016 ou posterior

1. No gerenciamento do AD FS, clique duas vezes em grupos de aplicativos e selecione **adicionar grupo de aplicativos**.

2. No Assistente de grupo do aplicativo, para o nome, digite **ADFSSSO** e em cliente-servidor de aplicativos, selecione o **aplicativo nativo acessando um aplicativo web** modelo. Clique em **Avançar**.

   ![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. Cópia de **identificador de cliente** valor.  Ele será usado posteriormente como o valor para ida: ClientId no arquivo de Web. config do aplicativo.

4. Insira o seguinte para **URI de redirecionamento:**  -  **https://localhost:44320/** .  Clique em **Adicionar**. Clique em **Avançar**.

   ![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. Sobre o **configurar a API da Web** tela, insira o seguinte para **identificador** -  **https://contoso.com/WebApp** .  Clique em **Adicionar**. Clique em **Avançar**.  Esse valor será usado posteriormente para **ida: ResourceID** no arquivo de Web. config do aplicativo.

   ![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. Sobre o **escolher política de controle de acesso** tela, selecione **permitir todos** e clique em **próxima**.

   ![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. No **configurar permissões do aplicativo** tela, verifique se **openid** e **allatclaims** estão selecionadas e clique em **Avançar**.

   ![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.png)

8. Sobre o **resumo** tela, clique em **próxima**.  

   ![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.png)

9. Sobre o **Complete** tela, clique em **fechar**.

10. No gerenciamento do AD FS, clique em grupos de aplicativos para obter a lista de todos os grupos de aplicativos. Clique duas vezes em **ADFSSSO** e selecione **propriedades**. Selecione **ADFSSSO - API Web** e clique em **editar...**

    ![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.png)

11. Na **ADFSSSO - propriedades da API Web** tela, selecione **regras de transformação de emissão** guia e clique em **Adicionar regra...**

    ![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.png)

12. Na **Adicionar Assistente de regra de declaração de transformação** tela, selecione **enviar declarações usando uma regra personalizada** na lista suspensa e clique em **Avançar**

    ![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.png)

13. Na **transformação de declaração assistente Adicionar regra** tela, insira **ForCustomIDToken** na **nome da regra de declaração** e a seguinte declaração de regra no **regra personalizada**. Clique em **concluir**

    ```  
    x:[]
    => issue(claim=x);  
    ```

    ![Remota](media/Custom-Id-Tokens-in-AD-FS/clientsnap10.png)

```

>[!NOTE]
>You can also use PowerShell to assign the allatclaims and openid scopes
>``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "[Client ID from #3 above]" -ServerRoleIdentifier "[Identifier from #5 above]" -ScopeNames "allatclaims","openid"
```

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-idtoken"></a>Baixe e modifique o aplicativo de exemplo para emitir declarações personalizadas no id_token

Esta seção discute como baixar o aplicativo Web de exemplo e modificá-lo no Visual Studio.   Vamos usar o exemplo do Azure AD que está [aqui](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Para baixar o projeto de exemplo, use Git Bash e digite o seguinte:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![OpenID do AD FS](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>Para modificar o aplicativo

1.  Abra o exemplo usando o Visual Studio.  

2.  Recompile o aplicativo para que todos os NuGets ausentes são restaurados.  

3.  Abra o arquivo Web. config.  Modifique os valores a seguir para que a aparência semelhante à seguinte:  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 above under section Create and configure an Application Group in AD FS 2016 or later]" />  
    <add key="ida:ResourceID" value="[Replace this with the Web API Identifier from #5 above]"  />
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with the Redirect URI from #4 above]" />  
    ```  

    ![OpenID do AD FS](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_2.PNG)  

4.  Abra o arquivo Startup.Auth.cs e faça as seguintes alterações:  

    -   Ajuste a lógica de inicialização de middleware OpenId Connect com as seguintes alterações:  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
        private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

    -   Comente o seguinte:  

            ```  
            //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
            ```

          ![OpenID do AD FS](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.PNG)

    -   Modificar ainda mais para baixo, as opções de middleware OpenId Connect da seguinte maneira:  

        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                Resource = resourceId,
                MetadataAddress = metadataAddress,  
                PostLogoutRedirectUri = postLogoutRedirectUri,
                RedirectUri = postLogoutRedirectUri
        ```  

        ![OpenID do AD FS](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_4.PNG)

5.  Abra o arquivo HomeController.cs e faça as seguintes alterações:  

    -   Adicione o seguinte:  

            ```  
            using System.Security.Claims;  
            ```

    -   Atualize o método About (), conforme mostrado abaixo:  

        ```  
        [Authorize]
        public ActionResult About()
        {
            ClaimsPrincipal cp = ClaimsPrincipal.Current;
            string userName = cp.FindFirst(ClaimTypes.WindowsAccountName).Value;
            ViewBag.Message = String.Format("Hello {0}!", userName);
            return View();
        }
        ```  

        ![OpenID do AD FS](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_5.PNG)

### <a name="test-the-custom-claims-in-id-token"></a>Testar as declarações personalizadas no token de ID

Depois que foram feitas as alterações acima, pressione F5. Isso abrirá a página de exemplo. Clique em entrar.

![OpenID do AD FS](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

Você será redirecionado para a página de entrada do AD FS. Vá em frente e entre.

![OpenID do AD FS](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

Quando essa operação for bem-sucedida, você deve ver o que você agora está conectado.

![OpenID do AD FS](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.PNG)

Clique em sobre o link. Você verá Hello [Username] que é recuperado da declaração de nome de usuário no token de ID

![OpenID do AD FS](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.PNG)

## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  
