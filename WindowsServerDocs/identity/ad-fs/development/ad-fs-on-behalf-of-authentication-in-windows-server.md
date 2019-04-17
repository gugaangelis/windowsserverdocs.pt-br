---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: "Criar um aplicativo de várias camadas usando On-Behalf-Of (OBO) usando o OAuth com o AD FS 2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8940cde2b78ce3ead499263e6fba0fbe28aae695
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016"></a>Criar um aplicativo de várias camadas usando On-Behalf-Of (OBO) usando o OAuth com o AD FS 2016

>Aplica-se a: Windows Server 2016

Este passo a passo fornece instruções para implementar uma autenticação de (OBO) no nome de usando o AD FS no Windows Server 2016 TP5.  O Saiba mais sobre autenticação OBO leia [AD FS cenários para desenvolvedores](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)

>Aviso: O exemplo que você pode criar aqui é apenas para fins educacionais. Estas instruções são para a implementação mais simples, mais mínima possível expor os elementos necessários do modelo. O exemplo não pode incluir todos os aspectos do tratamento de erros e outros se relacionam a funcionalidade e se concentra apenas recebendo uma autenticação bem-sucedida do OBO.

## <a name="overview"></a>Visão geral

Neste exemplo estamos criando um fluxo de autenticação em que um cliente acessará um serviço Web de camada intermediária e o serviço web, em seguida, vai atuar em nome do cliente para obter um token de acesso autenticado.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.PNG)

A seguir está o fluxo de autenticação que atinja a amostra
1. Cliente se autentica no ponto de extremidade de autorização do AD FS e solicita um código de autorização
2. Ponto de extremidade de autorização retorna o código de autenticação de cliente
3. Usa o código de autenticação de cliente e apresenta-as para o ponto de extremidade token AD FS ao token de acesso de solicitação de serviço de Web de camada intermediária como WebAPI
4. AD FS retorna o token de acesso ao serviço da Web de nível médio. Para obter funcionalidade adicional, o serviço de camada intermediária precisa acessar para o Backend WebAPI
5. Cliente usa o token de acesso para usar o serviço de camada intermediária.
6. Serviço de camada intermediário fornece o token de acesso do ponto final token do AD FS e solicitações de token de acesso para Backend WebAPI em nome de usuário autenticado
7. AD FS retorna o token de acesso para back-end WebAPI ao serviço de camada intermediária actiing como cliente
8. Serviço de camada intermediária usa o token de acesso fornecido pelo AD FS na etapa 7 para acessar o back-end WebAPI como cliente e executar as funções necessárias

## <a name="sample-structure"></a>Estrutura de amostra

Exemplo de comporão de três módulos


Módulo | Descrição
-------|------------
ToDoClient | Cliente nativo com o qual o usuário interage
ToDoService | Middle web nível API de que age como um cliente para o back-end WebAPI
WebAPIOBO | Api que é usado pelo ToDoService para executar a operação requisito quando o usuário adiciona um ToDoItem da web de back-end




## <a name="setting-up-the-development-box"></a>Configurando a caixa de desenvolvimento

Este passo a passo usa o Visual Studio 2015. O projeto usa intensamente a biblioteca de autenticação do Active Directory (ADAL). Para saber mais sobre ADAL leia [Active Directory autenticação biblioteca .NET](https://msdn.microsoft.com/library/azure/mt417579.aspx)

O exemplo também usa v 11.0 LocalDB SQL. Instale o SQL LocalDB antes trabalhando na amostra.

## <a name="setting-up-the-environment"></a>Configurando o ambiente
Trabalharemos com uma instalação básica do:

1. **DC**: controlador de domínio para o domínio em que será hospedado AD FS
2. **AD FS servidor**: AD FS servidor para o domínio
3. **Computador de desenvolvimento**: máquina onde podemos Visual Studio instalado e será ser desenvolvendo nossa amostra

Você pode usar se você quiser, apenas dois computadores. Um para DC/ADFS e outro para desenvolver a amostra.

Como configurar o controlador de domínio e o AD FS está além do escopo deste artigo. Para obter informações, consulte implantação adicionais:

- [Implantação do AD DS](../../ad-ds/deploy/AD-DS-Deployment.md)
- [AD FS implantação](../AD-FS-Deployment.md)

O exemplo é baseada na amostra de OBO existente contra Azure criada por Vittorio Bertocci e disponível [aqui](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof). Siga as instruções para clone o projeto no computador de desenvolvimento e criar uma cópia da amostra para começar a trabalhar com.

## <a name="clone-or-download-this-repository"></a>Clone ou baixar esse repositório

De um shell ou uma linha de comando:

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>Modificar o exemplo

Assim que você abre a solução WebAPI-OnBehalfOf-DotNet.sln, você notará que você tenha dois projetos na solução-

* **ToDoListClient**: ele funcionará como o cliente OpenID que o usuário interagirá com
* **ToDoListService**: isso serão o aplicativo de servidor Web de camada intermediária / serviço que irão interagir com outro back-end WebAPI OBO o usuário autenticado

Como você pode ver, precisamos adicionar outro projeto mais tarde que atuará como o recurso será acessado por ToDoListService a camada intermediária.

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>Configuração do AD FS para o cliente e o aplicativo de servidor Web

No formulário atual da amostra, a autenticação é configurada para ser feito contra do Azure AD. Queremos mudar o mecanismo de autenticação e direta-lo na direção do AD FS implantado no local. Para fazer isso, é necessário configurar o AD FS para reconhecer o cliente e servidor Web App temos no exemplo.

**Criar um grupo de aplicativos**

Abra o gerenciamento do AD FS MMC e adicione um novo grupo de aplicativos. Selecione modelo nativo-Application-WebAPI.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

Clique em Avançar e você verá a página para fornecer informações sobre o aplicativo cliente. Dê um nome apropriado para o aplicativo no AD FS do cliente. Copie o identificador de cliente e salve-o em algum lugar que você possa acessar mais tarde, isso será necessário na configuração do aplicativo no visual studio.

>Observação: O URI de redirecionamento pode ser qualquer URI arbitrária como ele realmente não é usado em caso de clientes nativos

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

Clique em Avançar e você verá a página para fornecer informações sobre WebAPI. Dê um nome para a entrada do AD FS adequado para o WebAPI e insira o URI de redirecionamento como o URI que você vê no Visual Studio para o ToDoListService

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

Clique em Avançar e você verá a página de política de controle de acesso escolher. Certifique-se de que você veja "Permitir que todos" na seção de política.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

Clique em Avançar e você receberá página Configurar permissões do aplicativo. Nessa página, selecione os escopos permitidos como openid (selecionada por padrão) e user_impersonation. O escopo 'user_impersonation' é necessário para poder com êxito solicitar um token de acesso em nome do AD FS.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

Clique em Avançar exibirá a página de resumo. Percorra o restante do assistente e concluir a configuração.

Para habilitar a autenticação em nome de, precisamos garantir que o AD FS retorna um token de acesso com escopo user_impersonation ao cliente. Modifique a emissão de declarações para ToDoListServiceWebApi incluir as seguintes regras personalizadas três:

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue open id scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "openid");

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**Adicionando ToDoListService como um cliente no grupo de aplicativos**

Nesse estágio, precisamos criar uma entrada adicional no AD FS para o servidor Web App atuar como um cliente e não apenas como um recurso. Abra o grupo de aplicativos que você acabou de criar e clique em Add Application.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

Você verá a página "Adicionar um novo aplicativo MySampleGroup". Nessa página, selecione "Aplicativo ou site de servidor" como o aplicativo autônomo

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

Clique em Avançar e você verá a página para fornecer detalhes do aplicativo. Forneça um nome para a entrada de configuração na seção nome adequado. Certifique-se de que o identificador de cliente é mesmo como o identificador para o ToDoListServiceWebAPI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

Clique em Avançar e você verá a página para configurar as credenciais do aplicativo. Clique em "Gerar um segredo compartilhado". Você receberá um segredo que é gerado automaticamente. Copie o segredo em algum local como isso será necessário enquanto configuramos o ToDoListService no visual studio.


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

Clique em Avançar e concluir o assistente.

### <a name="modifying-the-todolistclient-code"></a>Modificar o código de ToDoListClient

#### <a name="modify-the-application-config"></a>Modificar a configuração de aplicativo

Vá até você está no projeto ToDoListClient no WebAPI-OnBehalfOf-DotNet solução. Abra o arquivo App. config e fazer as seguintes modificações

* Comente a entrada da chave ida: locatário
* Para obter o ida: RedirectURI digitar o URI arbitrário que você forneceu ao configurar o MySampleGroup_ClientApplication no AD FS.
* Para a chave de ida: ClientID, forneça o cliente identificador de ID do AD FS deu ao configurar o MySampleGroup_ClientApplication.
* Para o ida: ToDoListResourceID forneça a ID do recurso que deu ao configurar o ToDoListServiceWebApi no AD FS
* Comente a chave ida: AADInstance
* Para o ida: ToDoListBaseAddress insira a ID de recurso da ToDoListServiceWebApi. Isso será usado ao chamar o ToDoList WebAPI.
* Adicione uma chave ida: autoridade e forneça o valor como o URI do AD FS.

Seu **appSettings** em App. config deve ser semelhante a este:

    <appSettings>
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />-->
    <add key="ida:ClientId" value="c7f7b85c-497c-4589-877f-b17a0bd13398" />
    <add key="ida:RedirectUri" value="https://arbitraryuri.com/" />
    <add key="ida:TodoListResourceId" value="https://localhost:44321/" />
    <!--<add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
    <add key="ida:TodoListBaseAddress" value="https://localhost:44321" />
    <add key="ida:Authority" value="https://fs.anandmsft.com/adfs/"/>
    </appSettings>

#### <a name="modifying-the-code"></a>Modificar o código

**MainWindow.xaml.cs**

Comente a linha lendo as informações de locatário de configuração do aplicativo

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

Altere o valor da autoridade de cadeia de caracteres para

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

Alterar o código para ler os valores corretos de ToDoListResourceId e ToDoListBaseAddress

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

Na função MainWindow() alterar a inicialização authcontext como:

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>Adicionar o recurso de back-end

Para concluir o fluxo em nome de, você precisa criar um recurso de back-end que acessará o ToDoListService em nome de usuário autenticado. A opção do recurso back-end pode variar conforme a necessidade, mas para fins de neste exemplo, você pode criar um WebAPI básico.

* Clique com o botão direito do mouse na solução 'WebAPI-OnBehalfOf-DotNet' no Gerenciador de soluções e selecione Adicionar -> Novo projeto
* Escolha o modelo de aplicativo Web ASP.NET

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* Na próxima solicitação clique em 'Alterar Authentication'
* Selecione 'Trabalho e contas de escola' e na lista suspensa direito 'Local'
* Inserir o caminho federationmetadata.xml para a implantação do AD FS e fornecer um URI de aplicativo (fornecer qualquer URI por enquanto, e você irá alterar mais tarde) e clique em Okey para adicionar o projeto à solução.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* Clique com botão direito controladores no Gerenciador de soluções, sob o novo projeto criado. Selecione Adicionar -> controlador
* A seleção de modelo, selecione 'API Web 2 controlador - Empty' e clique em Okey.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* Dê o nome de controlador apropriado

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* Adicione o seguinte código no controlador


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

Esse código simplesmente retornará a cadeia de caracteres quando alguém coloca uma solicitação Get para a WebAPI WebAPIOBO

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>Adicionar o novo back-end WebAPI ao AD FS

Abra o grupo de aplicativos MySampleGroup. Clique em Adicionar aplicativos e selecione modelo de API da Web e clique em Avançar.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

Na página Configurar API da Web fornece um nome apropriado para a entrada WebAPI e o identificador. O identificador deve ser o valor SSL URL de WebAPIOBO projeto no visual studio (semelhante ao que fizemos para BackendWebAPIAdfsAdd).

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

Continue o restante do assistente mesmo como quando configuramos o ToDoListService WebAPI. No final seu grupo de aplicativos deve se parecer com abaixo:

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>Modificar o código de ToDoListService

#### <a name="modifying-the-application-config"></a>Modificar a configuração de aplicativo

* Abra o arquivo Web. config
* Modifique as seguintes chaves

| Chave | Valor |
|:-----|:-------|
|público-alvo: ida| ID do ToDoListService conforme fornecido para o AD FS ao configurar ToDoListService WebAPI, por exemplo, https://localhost:44321/|
|ida: ClientID| ID do ToDoListService conforme fornecido para o AD FS ao configurar ToDoListService WebAPI, por exemplo, https://localhost:44321/ </br>**É muito importante que o público-alvo: ida e ida: ClientID correspondam uns aos outros**|
|ida: ClientSecret| Isso é o segredo do AD FS gerado quando você estava Configurando o cliente ToDoListService no AD FS|
|ida: ADFSMetadata| Esta é a URL para seus metadados AD FS, para https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml p. ex.|
|ida: OBOWebAPIBase| Esse é o endereço de base que usaremos para chamar o API de back-end para https://localhost:44300 p. ex.|
|ida: autoridade| Esta é a URL para o seu serviço AD FS, exemplo https://fs.anandmsft.com/adfs/|


Todos os outro ida: XXXXXXX teclas no **appsettings** nó pode ser convertido em comentário ou excluído

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

com

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

Adicione referência ao System.Web. Extensões. Modificar os membros da classe, substituindo o código a seguir

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

com

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The client secret is the credentials for the WebServer Client

    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

    // Base address of the WebAPI
    private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];

**Modifique a declaração usada para nome**

Do AD FS estamos disponibilizando reivindicação Nmae, mas não estamos emitindo declaração do identificador de nome. O exemplo usa o identificador de nome exclusivamente chave nos itens ToDo. Para simplificar, você pode remover com segurança o identificador de nome com a declaração de nome no código. Localizar e substituir todas as ocorrências do identificador de nome com nome.

**Modificar a rotina de Post e CallGraphAPIOnBehalfOfUser()**

Copie e cole o código abaixo no ToDoListController.cs e substitua o código de Post e CallGraphAPIOnBehalfOfUser

    // POST api/todolist
    public async Task Post(TodoItem todo)
    {
        if (!ClaimsPrincipal.Current.FindFirst("https://schemas.microsoft.com/identity/claims/scope").Value.Contains("user_impersonation"))
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
        //      The Resource ID of the service we want to call.
        //      The current user's access token, from the current request's authorization header.
        //      The credentials of this application.
        //      The username (UPN or email) of the user calling the API
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
                result = authContext.AcquireToken(OBOWebAPIBase, clientCred, userAssertion);
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


Por padrão, o visual studio é configurado para executar um projeto quando você atinge depurar para executar.

* Clique com o direito do mouse na solução e selecione Propriedades.
* Na página de propriedades selecione inicialização vários projetos e alterar a ação para iniciar para todas as três entradas.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

Pressione F5 e executar a solução

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

Clique no botão entrar. Você será solicitado a entrar na rede usando o AD FS

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

Depois que você entrar, adicione um item ToDo na lista. Nos bastidores, vamos fazer uma operação Post para o ToDoListService que ainda mais fará uma postagem na Web WebAPIOBO API.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

Na operação bem-sucedida, você verá que o item foi adicionado à lista com a mensagem adicional de API da Web que foi acessado por meio OBO auth-flow back-end.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

Você também pode ver os rastreamentos detalhados em Fiddler. Inicie o Fiddler e habilitar a descriptografia HTTPS. Você pode ver o que podemos fazer dois solicitações ao ponto de extremidade /adfs/oautincludes.
Na primeira interação, nós apresente o código de acesso para o ponto de extremidade do token e acessar um token para https://localhost:44321/ ![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

A segunda interação com o ponto de extremidade do token, você pode ver que temos **requested_token_use** definido como **on_behalf_of** e estamos usando o token de acesso obtido para o serviço web de camada intermediária, ou seja, https://localhost:44321/ como a declaração para obter o token em nome de.
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>Próximas etapas
[AD FS desenvolvimento](../../ad-fs/AD-FS-Development.md)  
