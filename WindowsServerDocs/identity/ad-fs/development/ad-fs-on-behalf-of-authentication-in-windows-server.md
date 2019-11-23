---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: Crie um aplicativo de várias camadas usando OBO (em nome de) usando o OAuth com o AD FS 2016 ou posterior
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9c6c6e7d2c12b6b822989bba05370015f7cd1833
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407817"
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016-or-later"></a>Crie um aplicativo de várias camadas usando OBO (em nome de) usando o OAuth com o AD FS 2016 ou posterior


Este tutorial fornece instruções para implementar uma autenticação em nome de (OBO) usando AD FS no Windows Server 2016 TP5 ou posterior. Para saber mais sobre a autenticação OBO, leia [AD FS cenários de aplicativos e fluxos do OpenID Connect/OAuth](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)

>Aviso: o exemplo que você pode criar aqui é apenas para fins educacionais. Essas instruções são para a implementação mais simples e mínima possível para expor os elementos necessários do modelo. O exemplo pode não incluir todos os aspectos do tratamento de erros e outras funcionalidades relacionadas e se concentrar apenas em obter uma autenticação OBO bem-sucedida.

## <a name="overview"></a>Visão geral

Neste exemplo, criaremos um fluxo de autenticação em que um cliente acessará um serviço Web de camada intermediária e o serviço Web agirá em nome do cliente autenticado para obter um token de acesso.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.png)

Abaixo está o fluxo de autenticação que o exemplo obterá
1. O cliente é autenticado para AD FS ponto de extremidade de autorização e solicita um código de autorização
2. O ponto de extremidade de autorização retorna o código de autenticação para o cliente
3. O cliente usa o código de autenticação e o apresenta ao ponto de extremidade do token de AD FS para solicitar o token de acesso para o serviço Web da camada intermediária como WebAPI
4. AD FS retorna o token de acesso para o serviço Web da camada intermediária. Para funcionalidade adicional, o serviço de camada intermediária precisa acessar o WebAPI de back-end
5. O cliente usa o token de acesso para usar o serviço de camada intermediária.
6. O serviço de camada intermediária fornece o token de acesso para o ponto de extremidade de token AD FS e solicita o token de acesso para WebAPI de back-end em nome do usuário autenticado
7. AD FS retorna o token de acesso para WebAPI de back-end para o serviço de camada intermediária actiing como cliente
8. O serviço de camada intermediária usa o token de acesso fornecido por AD FS na etapa 7 para acessar o WebAPI de back-end como cliente e executar as funções necessárias

## <a name="sample-structure"></a>Estrutura de exemplo

O exemplo incluirá três módulos


Módulo | Descrição
-------|------------
ToDoClient | Cliente nativo com o qual o usuário interage
ToDoService | API Web da camada intermediária que atua como um cliente para o back-end WebAPI
WebAPIOBO | API Web de back-end usada pelo ToDoService para executar a operação de requisito quando o usuário adiciona um ToDoItem




## <a name="setting-up-the-development-box"></a>Configurando a caixa de desenvolvimento

O passo a passo usa o Visual Studio 2015. O projeto usa intensamente o Biblioteca de Autenticação do Active Directory (ADAL). Para saber mais sobre a ADAL, leia [biblioteca de autenticação do Active Directory .net](https://msdn.microsoft.com/library/azure/mt417579.aspx)

O exemplo também usa o SQL LocalDB v 11.0. Instale o SQL LocalDB antes de trabalhar no exemplo.

## <a name="setting-up-the-environment"></a>Configurando o ambiente
Trabalharemos com uma configuração básica do:

1. **DC**: controlador de domínio para o domínio no qual AD FS será hospedado
2. **Servidor de AD FS**: o servidor de AD FS para o domínio
3. **Máquina de desenvolvimento**: computador onde o Visual Studio foi instalado e desenvolverei nosso exemplo

Você pode, se desejar, usar apenas duas máquinas. Um para DC/ADFS e outro para desenvolver o exemplo.

Como configurar o controlador de domínio e AD FS está além do escopo deste artigo. Para obter informações adicionais de implantação, consulte:

- [Implantação do AD DS](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Implantação do AD FS](../AD-FS-Deployment.md)

O exemplo é baseado no exemplo de OBO existente no Azure criado pelo Vittorio Bertocci e disponível [aqui](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof). Siga as instruções para clonar o projeto em seu computador de desenvolvimento e criar uma cópia do exemplo para começar a trabalhar com o.

## <a name="clone-or-download-this-repository"></a>Clonar ou baixar este repositório

No Shell ou na linha de comando:

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>Modificando o exemplo

Assim que abrir a solução WebAPI-OnBehalfOf-DotNet. sln, você observará que tem dois projetos na solução

* **ToDoListClient**: Isso servirá como o cliente OpenID com o qual o usuário será interagindo
* **ToDoListService**: este será o aplicativo/serviço do servidor de camada intermediária que irá interagir com outro WebAPI de back-end obo o usuário autenticado

Como você pode ver, precisaremos adicionar outro projeto posteriormente, que atuará como o recurso que será acessado pelo ToDoListService da camada intermediária.

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>Configurando AD FS para o aplicativo do cliente e do webserver

Na forma atual do exemplo, a autenticação é configurada para ser feita no Azure AD. Queremos alterar o mecanismo de autenticação e direcioná-lo para AD FS implantado localmente. Para fazer isso, precisamos configurar AD FS para reconhecer o aplicativo do cliente e do WebServer que temos no exemplo.

**Criando um grupo de aplicativos**

Abra o MMC de gerenciamento de AD FS e adicione um novo grupo de aplicativos. Selecione o modelo WebAPI de aplicativo nativo.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

Clique em avançar e você verá a página para fornecer informações sobre o aplicativo cliente. Forneça um nome apropriado para o aplicativo cliente no AD FS. Copie o identificador de cliente e salve-o em algum lugar que você possa acessar posteriormente, pois isso será necessário na configuração do aplicativo no Visual Studio.

>Observação: o URI de redirecionamento pode ser qualquer URI arbitrário, pois ele realmente não é usado no caso de clientes nativos

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

Clique em avançar e você verá a página para fornecer informações sobre WebAPI. Forneça um nome adequado para a entrada de AD FS para o WebAPI e insira o URI de redirecionamento como o URI que você vê no Visual Studio para o ToDoListService

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

Clique em avançar e você verá a página escolher política de controle de acesso. Verifique se você vê "permitir todos" na seção de política.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

Clique em avançar e a página configurar permissões de aplicativo será exibida. Nessa página, selecione os escopos permitidos como OpenID (selecionados por padrão) e user_impersonation. O escopo ' user_impersonation ' é necessário para poder solicitar com êxito um token de acesso em nome de AD FS.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

Clique em avançar para exibir a página Resumo. Percorra o restante do assistente e conclua a configuração.

Para habilitar a autenticação em nome de, precisamos garantir que AD FS retorna um token de acesso com user_impersonation de escopo ao cliente. Modifique a emissão de declarações para ToDoListServiceWebApi para incluir as três seguintes regras personalizadas:

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**Adicionando ToDoListService como um cliente no grupo de aplicativos**

Neste estágio, precisamos fazer uma entrada adicional em AD FS para que o aplicativo WebServer atue como um cliente e não apenas como um recurso. Abra o grupo de aplicativos que você acabou de criar e clique em Adicionar aplicativo.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

Você verá a página "adicionar um novo aplicativo ao mysampleo". Nessa página, selecione "aplicativo de servidor ou site" como o aplicativo autônomo

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

Clique em avançar e você verá a página para fornecer detalhes do aplicativo. Forneça um nome adequado para a entrada de configuração na seção nome. Verifique se o identificador do cliente é o mesmo que o identificador para o ToDoListServiceWebAPI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

Clique em avançar e você verá a página para configurar as credenciais do aplicativo. Clique em "gerar um segredo compartilhado". Será exibido um segredo que é gerado automaticamente. Copie o segredo em algum local, pois isso será necessário enquanto configuramos o ToDoListService no Visual Studio.


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

Clique em avançar e conclua o assistente.

### <a name="modifying-the-todolistclient-code"></a>Modificando o código ToDoListClient

#### <a name="modify-the-application-config"></a>Modificar a configuração do aplicativo

Vá para você é o projeto ToDoListClient na solução WebAPI-OnBehalfOf-DotNet. Abra o arquivo app. config e faça as seguintes modificações

* Comente a entrada de chave ida: Tenant
* Para o ida: RedirectURI, insira o URI arbitrário que você forneceu ao configurar o MySampleGroup_ClientApplication no AD FS.
* Para a chave ida: ClientID, forneça o identificador de ID do cliente que AD FS fornecido ao configurar o MySampleGroup_ClientApplication.
* Para o ida: ToDoListResourceID forneça a ID de recurso que você forneceu ao configurar o ToDoListServiceWebApi no AD FS
* Comente a chave ida: AADInstance
* Para o ida: ToDoListBaseAddress, insira a ID de recurso do ToDoListServiceWebApi. Isso será usado ao chamar o WebAPI de ToDolist.
* Adicione uma autoridade de chave ida: e forneça o valor como o URI para AD FS.

Suas **appSettings** em app. config devem ser semelhantes a:

    <appSettings>
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />-->
    <add key="ida:ClientId" value="c7f7b85c-497c-4589-877f-b17a0bd13398" />
    <add key="ida:RedirectUri" value="https://arbitraryuri.com/" />
    <add key="ida:TodoListResourceId" value="https://localhost:44321/" />
    <!--<add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
    <add key="ida:TodoListBaseAddress" value="https://localhost:44321" />
    <add key="ida:Authority" value="https://fs.anandmsft.com/adfs/"/>
    </appSettings>

#### <a name="modifying-the-code"></a>Modificando o código

**MainWindow.xaml.cs**

Comentar a linha que lê as informações do locatário na configuração do aplicativo

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

Alterar o valor da autoridade de cadeia de caracteres para

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

Altere o código para ler os valores corretos de ToDoListResourceId e ToDoListBaseAddress

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

Na função MainWindow (), altere a inicialização do authcontext como:

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>Adicionando o recurso de back-end

Para concluir o fluxo em nome de, você precisa criar um recurso de back-end que o ToDoListService acessará em nome do usuário autenticado. A escolha do recurso de back-end pode variar de acordo com o requisito, mas, para fins deste exemplo, você pode criar um WebAPI básico.

* Clique com o botão direito na solução ' WebAPI-OnBehalfOf-DotNet ' no Gerenciador de soluções e selecione Adicionar > novo projeto
* Escolher modelo de aplicativo Web ASP.NET

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* Na próxima solicitação, clique em ' alterar autenticação '
* Selecione ' contas corporativas e de estudante ' e, na lista suspensa à direita, selecione ' local '
* Insira o caminho FederationMetadata. xml para sua implantação de AD FS e forneça um URI de aplicativo (forneça qualquer URI por enquanto, e você o alterará posteriormente) e clique em OK para adicionar o projeto à solução.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* Clique com o botão direito do mouse em controladores no Gerenciador de soluções no novo projeto criado. Selecionar controlador de > de adição
* Na seleção de modelo, selecione "controlador da Web API 2-vazio" e clique em OK.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* Fornecer o nome do controlador apropriado

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* Adicione o seguinte código ao controlador


~~~
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http;
    namespace WebAPIOBO.Controllers
    {
        public class WebAPIOBOController : ApiController
        {
            public IHttpActionResult Get()
            {
                return Ok("WebAPI via OBO");
            }
        }
    }
~~~

Esse código simplesmente retornará a cadeia de caracteres quando alguém colocar uma solicitação get para o WebAPI WebAPIOBO

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>Adicionando o novo WebAPI de back-end para AD FS

Abra o grupo de aplicativos MySample. Clique em Adicionar aplicativo e selecione modelo de API Web e clique em Avançar.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

Na página Configurar API Web, forneça um nome apropriado para a entrada WebAPI e o identificador. O identificador deve ser o valor URL SSL do projeto WebAPIOBO no Visual Studio (semelhante ao que fizemos para BackendWebAPIAdfsAdd).

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

Continue com o restante do assistente, como quando configuramos o ToDoListService WebAPI. No final, seu grupo de aplicativos deve ter a seguinte aparência:

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>Modificando o código ToDoListService

#### <a name="modifying-the-application-config"></a>Modificando a configuração do aplicativo

* Abra o arquivo Web. config
* Modificar as seguintes chaves

| Chave                      | Valor                                                                                                                                                                                                                   |
|:-------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ida: público             | ID do ToDoListService, conforme fornecido para AD FS ao configurar o WebAPI ToDoListService, por exemplo, https://localhost:44321/                                                                                         |
| Ida: ClientID             | ID do ToDoListService, conforme fornecido para AD FS ao configurar o WebAPI ToDoListService, por exemplo, <https://localhost:44321/> </br>**É muito importante que o ida: Audience e Ida: ClientID correspondam um ao outro** |
| Ida: ClientSecret         | Esse é o segredo que AD FS gerado quando você estava Configurando o cliente ToDoListService no AD FS                                                                                                                   |
| Ida: AdfsMetadataEndpoint | Essa é a URL para seus metadados de AD FS, por exemplo, https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml                                                                                             |
| Ida: OBOWebAPIBase        | Esse é o endereço base que usaremos para chamar a API de back-end, por exemplo, https://localhost:44300                                                                                                                     |
| Ida: Authority            | Esta é a URL para seu serviço de AD FS, exemplo https://fs.anandmsft.com/adfs/                                                                                                                                          |

Todas as outras chaves ida: XXXXXXX no nó **appSettings** podem ser comentadas ou excluídas

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>Alterar a autenticação do Azure AD para o AD FS

* Abra o arquivo Startup.Auth.cs
* Remova o código a seguir

        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                TokenValidationParameters = new TokenValidationParameters{ SaveSigninToken = true }
            });

with

        app.UseActiveDirectoryFederationServicesBearerAuthentication(
            new ActiveDirectoryFederationServicesBearerAuthenticationOptions
            {
                MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
                TokenValidationParameters = new TokenValidationParameters()
                {
                    SaveSigninToken = true,
                    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"]
                }
            });

#### <a name="modifying-the-todolistcontroller"></a>Modificando o ToDoListController

Adicione referência a System. Web. Extensions. Modifique os membros da classe substituindo o código abaixo

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The App Key is a credential used by the application to authenticate to Azure AD.
    // The Tenant is the name of the Azure AD tenant in which this application is registered.
    // The AAD Instance is the instance of Azure, for example public Azure or Azure China.
    // The Authority is the sign-in URL of the tenant.
    //
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string appKey = ConfigurationManager.AppSettings["ida:AppKey"];

    //
    // To authenticate to the Graph API, the app needs to know the Grah API's App ID URI.
    // To contact the Me endpoint on the Graph API we need the URL as well.
    //
    private static string graphResourceId = ConfigurationManager.AppSettings["ida:GraphResourceId"];
    private static string graphUserUrl = ConfigurationManager.AppSettings["ida:GraphUserUrl"];
    private const string TenantIdClaimType = "https://schemas.microsoft.com/identity/claims/tenantid";

with

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The client secret is the credentials for the WebServer Client

    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

    // Base address of the WebAPI
    private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];

**Modificar a declaração usada para o nome**

De AD FS estamos emitindo a declaração Nmae, mas não estamos emitindo a declaração NameIdentifier. O exemplo usa NameIdentifier para a chave exclusiva nos itens de tarefas. Para simplificar, você pode remover com segurança o NameIdentifier com a declaração de nome no código. Localizar e substituir todas as ocorrências de NameIdentifier por nome.

**Modificar rotina de postagem e CallGraphAPIOnBehalfOfUser ()**

Copie e cole o código abaixo em ToDoListController.cs e substitua o código para post e CallGraphAPIOnBehalfOfUser

    // POST api/todolist
    public async Task Post(TodoItem todo)
    {
      if (!ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value.Contains("user_impersonation"))
        {
            throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found" });
        }

      //
      // Call the WebAPIOBO On Behalf Of the user who called the To Do list web API.
      //

      string augmentedTitle = null;
      string custommessage = await CallGraphAPIOnBehalfOfUser();

      if (custommessage != null)
        {
            augmentedTitle = String.Format("{0}, Message: {1}", todo.Title, custommessage);
        }
        else
        {
            augmentedTitle = todo.Title;
        }

      if (null != todo && !string.IsNullOrWhiteSpace(todo.Title))
        {
            db.TodoItems.Add(new TodoItem { Title = augmentedTitle, Owner = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value });
            db.SaveChanges();
        }
      }

      public static async Task<string> CallGraphAPIOnBehalfOfUser()
      {
        string accessToken = null;
        AuthenticationResult result = null;
        AuthenticationContext authContext = null;
        HttpClient httpClient = new HttpClient();
        string custommessage = "";

        //
        // Use ADAL to get a token On Behalf Of the current user.  To do this we will need:
        // The Resource ID of the service we want to call.
        // The current user's access token, from the current request's authorization header.
        // The credentials of this application.
        // The username (UPN or email) of the user calling the API
        //

        ClientCredential clientCred = new ClientCredential(clientId, clientSecret);
        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        string userName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn) != null ? ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value : ClaimsPrincipal.Current.FindFirst(ClaimTypes.Email).Value;
        string userAccessToken = bootstrapContext.Token;
        UserAssertion userAssertion = new UserAssertion(bootstrapContext.Token, "urn:ietf:params:oauth:grant-type:jwt-bearer", userName);

        string userId = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
        authContext = new AuthenticationContext(authority, false);

        // In the case of a transient error, retry once after 1 second, then abandon.
        // Retrying is optional.  It may be better, for your application, to return an error immediately to the user and have the user initiate the retry.
        bool retry = false;
        int retryCount = 0;

        do
          {
              retry = false;
              try
                {
                    result = await authContext.AcquireTokenAsync(OBOWebAPIBase, clientCred, userAssertion);
                    //result = await authContext.AcquireTokenAsync(...);
                    accessToken = result.AccessToken;
                }
              catch (AdalException ex)
                {
                    if (ex.ErrorCode == "temporarily_unavailable")
                    {
                        // Transient error, OK to retry.
                        retry = true;
                        retryCount++;
                        Thread.Sleep(1000);
                    }
                }
          } while ((retry == true) && (retryCount < 1));

        if (accessToken == null)
          {
              // An unexpected error occurred.
              return (null);
          }

        // Once the token has been returned by ADAL, add it to the http authorization header, before making the call to access the To Do list service.
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

        // Call the WebAPIOBO.
        HttpResponseMessage response = await httpClient.GetAsync(OBOWebAPIBase + "/api/WebAPIOBO");


        if (response.IsSuccessStatusCode)
          {
              // Read the response and databind to the GridView to display To Do items.
              string s = await response.Content.ReadAsStringAsync();
              JavaScriptSerializer serializer = new JavaScriptSerializer();
              custommessage = serializer.Deserialize<string>(s);
              return custommessage;
          }
        else
          {
              custommessage = "Unsuccessful OBO operation : " + response.ReasonPhrase;
          }
        // An unexpected error occurred calling the Graph API.  Return a null profile.
        return (null);
    }

## <a name="running-the-solution"></a>Executando a solução


Por padrão, o Visual Studio é configurado para executar um projeto quando você clica em Depurar para executar.

* Clique com o botão direito do mouse na solução e selecione Propriedades.
* Na página Propriedades, selecione vários projetos de inicialização e altere a ação para iniciar para todas as três entradas.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

Pressione F5 e execute a solução

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

Clique no botão entrar. Você será solicitado a entrar usando AD FS

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

Depois de entrar, adicione um item de tarefas pendentes na lista. Nos bastidores, vamos fazer uma operação post para o ToDoListService que fará mais uma postagem na API Web do WebAPIOBO.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

Na operação bem-sucedida, você verá que o item foi adicionado à lista com a mensagem adicional da API Web de back-end que foi acessada usando o fluxo de autenticação OBO.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

Você também pode ver os rastreamentos detalhados sobre o Fiddler. Inicie o Fiddler e habilite a descriptografia de HTTPS. Você pode ver que fazemos duas solicitações para o ponto de extremidade/ADFS/oautincludes.
Na primeira interação, apresentamos o código de acesso ao ponto de extremidade do token e obtemos um token de acesso para https://localhost:44321/ ![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

Na segunda interação com o ponto de extremidade do token, você pode ver que temos **requested_token_use** definido como **on_behalf_of** e estamos usando o token de acesso obtido para o serviço Web de camada intermediária, ou seja, https://localhost:44321/ como a declaração para obter o token em nome de.
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  
