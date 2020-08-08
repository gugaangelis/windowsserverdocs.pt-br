---
title: Personalize as declarações a serem emitidas no id_token ao usar o OpenID Connect ou o OAuth com o AD FS 2016 ou posterior
description: Uma visão geral técnica dos conceitos de token de ID personalizada no AD FS 2016 ou posterior
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 04/29/2020
ms.topic: article
ms.reviewer: anandy
ms.openlocfilehash: 9ab9a22e471a576a2632e3dbb054d21dd4534e46
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940544"
---
# <a name="customize-claims-to-be-emitted-in-id_token-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>Personalize as declarações a serem emitidas no id_token ao usar o OpenID Connect ou o OAuth com o AD FS 2016 ou posterior

## <a name="overview"></a>Visão geral

O artigo [aqui](native-client-with-ad-fs.md) mostra como criar um aplicativo que usa AD FS para logon do OpenID Connect. No entanto, por padrão, há apenas um conjunto fixo de declarações disponíveis no id_token. AD FS 2016 e versões posteriores têm a capacidade de personalizar o id_token em cenários do OpenID Connect.

## <a name="when-are-custom-id-tokens-used"></a>Quando são usados tokens de ID personalizada?

Em determinados cenários, é possível que o aplicativo cliente não tenha um recurso que ele está tentando acessar. Portanto, não precisa realmente de um token de acesso. Nesses casos, o aplicativo cliente basicamente precisa apenas de um token de ID, mas com algumas declarações adicionais para ajudar na funcionalidade.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>Quais são as restrições para obter declarações personalizadas no token de ID?

### <a name="scenario-1"></a>Cenário 1

![Restringir](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1. `response_mode`é definido como`form_post`
2. Somente clientes públicos podem obter declarações personalizadas no token de ID
3. O identificador de terceira parte confiável (identificador de API da Web) deve ser o mesmo que o identificador de cliente

### <a name="scenario-2"></a>Cenário 2

![Restringir](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Com a atualização de segurança do [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) ou posterior instalada em seus servidores de AD FS
1. `response_mode`é definido como form_post
2. Os clientes públicos e confidenciais podem obter declarações personalizadas no token de ID
3. Atribua `allatclaims` o escopo ao par cliente – RP.

Você pode atribuir o escopo usando o `Grant-ADFSApplicationPermission` cmdlet, conforme indicado no exemplo abaixo:

```powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Criando e configurando um aplicativo OAuth para lidar com declarações personalizadas no token de ID

Siga as etapas abaixo para criar e configurar o aplicativo no AD FS para receber o token de ID com declarações personalizadas.

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>Criar e configurar um grupo de aplicativos no AD FS 2016 ou posterior

1. Em gerenciamento de AD FS, clique com o botão direito do mouse em grupos de aplicativos e selecione **Adicionar grupo de aplicativos**.
2. No assistente de grupo de aplicativos, para o nome, insira **ADFSSSO** e, em aplicativos cliente-servidor, selecione o **aplicativo nativo acessando um modelo de aplicativo Web** . Clique em **Próximo**.

   ![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. Copie o valor do **identificador de cliente** .  Ele será usado posteriormente como o valor de ida: ClientId no arquivo de web.config de aplicativos.
4. Insira o seguinte para o **URI de redirecionamento:**  -  **https://localhost:44320/** .  Clique em **Adicionar**. Clique em **Próximo**.

   ![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. Na tela **Configurar API da Web** , insira o seguinte para o **identificador**  -  **https://contoso.com/WebApp** .  Clique em **Adicionar**. Clique em **Próximo**.  Esse valor será usado posteriormente para **ida: ResourceId** no arquivo de web.config de aplicativos.

   ![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. Na tela **escolher política de controle de acesso** , selecione **permitir todos** e clique em **Avançar**.

   ![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. Na tela **configurar permissões de aplicativo** , verifique se **OpenID** e **allatclaims** estão selecionados e clique em **Avançar**.

   ![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.PNG)

8. Na tela **Resumo** , clique em **Avançar**.

   ![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.PNG)

9. Na tela **concluir** , clique em **fechar**.
10. Em gerenciamento de AD FS, clique em grupos de aplicativos para obter a lista de todos os grupos de aplicativos. Clique com o botão direito do mouse em **ADFSSSO** e selecione **Propriedades**. Selecione **ADFSSSO-API da Web** e clique em **Editar..** .

    ![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.PNG)

11. Na tela **ADFSSSO – Propriedades da API Web** , selecione a guia **regras de transformação de emissão** e clique em **Adicionar regra..** .

    ![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.PNG)

12. Na tela **Adicionar Assistente de regra de declaração de transformação** , selecione **enviar declarações usando uma regra personalizada** na lista suspensa e clique em **Avançar**

    ![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.PNG)

13. Na tela **Adicionar Assistente de regra de declaração de transformação** , insira **ForCustomIDToken** no nome da **regra de declaração** e a regra de declaração a seguir na **regra personalizada**. Clique em **Concluir**

    ```
    x:[]
    => issue(claim=x);
    ```

    ![Cliente](media/Custom-Id-Tokens-in-AD-FS/clientsnap10.PNG)

    > [!NOTE]
    > Você também pode usar o PowerShell para atribuir `allatclaims` os `openid` escopos e.

    ``` powershell
    Grant-AdfsApplicationPermission -ClientRoleIdentifier "[Client ID from #3 above]" -ServerRoleIdentifier "[Identifier from #5 above]" -ScopeNames "allatclaims","openid"
    ```

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-id_token"></a>Baixar e modificar o aplicativo de exemplo para emitir declarações personalizadas no id_token

Esta seção discute como baixar o aplicativo Web de exemplo e modificá-lo no Visual Studio. Usaremos o exemplo do Azure AD localizado [aqui](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC).

Para baixar o projeto de exemplo, use git bash e digite o seguinte:

```
git clone https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC
```

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>Para modificar o aplicativo

1. Abra o exemplo usando o Visual Studio.
2. Recompile o aplicativo para que todos os NuGets ausentes sejam restaurados.
3. Abra o arquivo web.config.  Modifique os valores a seguir para que a aparência seja parecida com a seguinte:

   ```xml
   <add key="ida:ClientId" value="[Replace this Client Id from #3 above under section Create and configure an Application Group in AD FS 2016 or later]" />
   <add key="ida:ResourceID" value="[Replace this with the Web API Identifier from #5 above]"  />
   <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />
   <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />
   <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
   <add key="ida:PostLogoutRedirectUri" value="[Replace this with the Redirect URI from #4 above]" />
   ```

   ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_2.png)

4. Abra o arquivo Startup.Auth.cs e faça as seguintes alterações:

   - Ajuste a lógica de inicialização do middleware OpenId Connect com as seguintes alterações:

      ```cs
      private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
      //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
      //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
      private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
      private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
      private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];
      ```

   - Comente o seguinte:

      ```cs
      //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);
      ```

      ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.png)

   - Além disso, modifique as opções de middleware do OpenId Connect, como no seguinte:

      ```cs
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

      ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_4.png)

5. Abra o arquivo HomeController.cs e faça as seguintes alterações:

   - Adicione o seguinte:

      ```cs
      using System.Security.Claims;
      ```

   - Atualize o `About()` método, conforme mostrado abaixo:

      ```cs
      [Authorize]
      public ActionResult About()
      {
          ClaimsPrincipal cp = ClaimsPrincipal.Current;
          string userName = cp.FindFirst(ClaimTypes.WindowsAccountName).Value;
          ViewBag.Message = String.Format("Hello {0}!", userName);
          return View();
      }
      ```

      ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_5.png)

### <a name="test-the-custom-claims-in-id-token"></a>Testar as declarações personalizadas no token de ID

Depois que as alterações acima tiverem sido feitas, pressione F5. Isso abrirá a página de exemplo. Clique em entrar.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

Você será redirecionado para a página de entrada do AD FS. Vá em frente e entre.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

Quando isso for bem-sucedido, você verá que agora está conectado.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.png)

Clique no link Sobre . Você verá "Olá [username]", que é recuperado da declaração de nome de usuário no token de ID

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.png)

## <a name="next-steps"></a>Próximas etapas

[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)
