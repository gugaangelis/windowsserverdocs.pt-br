---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: Criar um aplicativo de várias camadas usando em nome (OBO) usando o OAuth com o AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 33d0bfa4139f16c90f3d79f5b61188b4d311538b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858937"
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016"></a>Criar um aplicativo de várias camadas usando em nome (OBO) usando o OAuth com o AD FS 2016

>Aplica-se a: Windows Server 2016

Este passo a passo fornece instruções para implementar uma on-behalf-of (OBO) a autenticação usando o AD FS no Windows Server 2016 TP5. Para saber mais sobre a autenticação OBO, leia [cenários do AD FS para desenvolvedores](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)

>AVISO: O exemplo que você pode criar aqui é apenas para fins educacionais. Essas instruções são para a implementação mais simples e mais concisa possível expor os elementos necessários do modelo. O exemplo pode não incluir todos os aspectos de tratamento de erros e outros se relacionam com a funcionalidade e se concentra somente na obtenção de uma autenticação bem-sucedida do OBO.

## <a name="overview"></a>Visão geral

Neste exemplo estamos criando um fluxo de autenticação em que um cliente estarão acessando um serviço Web de camada intermediária e o serviço web, em seguida, irá atuar em nome do cliente autenticado para obter um token de acesso.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.png)

Abaixo está o fluxo de autenticação que o exemplo atinja
1. Cliente se autentica no ponto de extremidade de autorização do AD FS e solicita um código de autorização
2. Ponto de extremidade de autorização retorna o código de autenticação para cliente
3. Usa o código de autenticação de cliente e apresenta-as para o ponto de extremidade token do AD FS para solicitar o token de acesso para o serviço de Web de camada intermediária como WebAPI
4. AD FS retorna o token de acesso ao serviço Web de camada intermediária. Para obter funcionalidade adicional, o serviço de camada intermediária precisa acessar o Backend WebAPI
5. Cliente usa o token de acesso para usar o serviço de camada intermediária.
6. Serviço de camada intermediário fornece o token de acesso para o ponto de extremidade de token do AD FS e solicitações de token de acesso para Backend WebAPI no nome de usuário autenticado
7. AD FS retorna o token de acesso para o back-end WebAPI para o serviço de camada intermediária actiing como cliente
8. Serviço de camada intermediária usa o token de acesso fornecido pelo AD FS na etapa 7 para acessar o back-end da API Web como cliente e realizar as funções necessárias

## <a name="sample-structure"></a>Exemplo de estrutura

Exemplo será composta por três módulos


Módulo | Descrição
-------|------------
ToDoClient | Cliente nativo com o qual o usuário interage
ToDoService | Intermediária camada API da web que atua como um cliente para o back-end WebAPI
WebAPIOBO | Back-end da web api que é usada pelo ToDoService para executar a operação necessária quando o usuário adiciona um ToDoItem




## <a name="setting-up-the-development-box"></a>Como configurar a caixa de desenvolvimento

Este passo a passo usa o Visual Studio 2015. O projeto usa intensamente a biblioteca de autenticação do Active Directory (ADAL). Para saber mais sobre a ADAL, leia [.NET da biblioteca autenticação do Active Directory](https://msdn.microsoft.com/library/azure/mt417579.aspx)

O exemplo também usa o LocalDB do SQL v11.0. Instale o LocalDB do SQL antes de trabalhar na amostra.

## <a name="setting-up-the-environment"></a>Configurando o ambiente
Trabalharemos com uma configuração básica de:

1. **DC**: Controlador de domínio para o domínio no qual o AD FS será hospedado.
2. **Servidor do AD FS**: O servidor do AD FS para o domínio
3. **Máquina de desenvolvimento**: Computador em que podemos ter instalado o Visual Studio e desenvolverá nossa amostra

Você pode usar se desejar, somente duas máquinas. Um para DC/AD FS e o outro para o desenvolvimento de exemplo.

Como configurar o controlador de domínio e o AD FS está além do escopo deste artigo. Para obter informações adicionais de implantação consulte:

- [Implantação do AD DS](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Implantação do AD FS](../AD-FS-Deployment.md)

O exemplo é disponíveis e com base no exemplo OBO existente no Azure criado por Vittorio Bertocci [aqui](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof). Siga as instruções para clonar o projeto em seu computador de desenvolvimento e criar uma cópia da amostra para começar a trabalhar com.

## <a name="clone-or-download-this-repository"></a>Clonar ou baixar este repositório

No shell ou na linha de comando:

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>Modificando o exemplo

Assim que você abre a solução OnBehalfOf-WebAPI-DotNet.sln, você observará que você tenha dois projetos na solução-

* **ToDoListClient**: Isso servirá como o cliente de OpenID que o usuário interagirá com
* **ToDoListService**: Isso será ser o aplicativo de servidor Web de camada intermediária / de serviço que irá interagir com outro back-end WebAPI OBO o usuário autenticado

Como você pode ver, precisamos adicionar outro projeto mais tarde que atuará como o recurso que será acessado por ToDoListService camada intermediária.

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>Configuração do AD FS para o cliente e o aplicativo de servidor Web

No formulário de exemplo atual, a autenticação é configurada para ser feito no AD do Azure. Queremos alterar o mecanismo de autenticação e direct-lo na direção do AD FS implantado no local. Para fazer isso, precisamos configurar o AD FS para reconhecer o cliente e o App WebServer temos no exemplo.

**Criar um grupo de aplicativos**

Abra o gerenciamento do AD FS MMC e adicione um novo grupo de aplicativos. Selecionar modelo de aplicativo-WebAPI-nativo.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

Clique em Avançar e você verá a página para fornecer informações sobre o aplicativo cliente. Dê um nome apropriado para o aplicativo no AD FS do cliente. Copie o identificador de cliente e salve-o em algum lugar, que você pode acessar posteriormente, pois isso será necessária a configuração de aplicativo no visual studio.

>Observação: O URI de redirecionamento pode ser qualquer URI arbitrário, pois ele realmente não é usado no caso de clientes nativos

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

Clique em Avançar e você verá a página para fornecer informações sobre a API da Web. Dê um nome adequado para a entrada do AD FS para a API da Web e insira o URI de redirecionamento como o URI que você vê no Visual Studio para o projeto ToDoListService

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

Clique em Avançar e você verá a página de política de controle de acesso escolher. Certifique-se de que você vê a opção "Permitir todos" na seção de política.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

Clique em Avançar e você verá a página de permissões do aplicativo configurar. Nessa página, selecione os escopos permitidos como openid (selecionado por padrão) e user_impersonation. O escopo 'user_impersonation' é necessário para poder com êxito solicitar um token de acesso em nome do AD FS.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

Em seguida clique exibirá a página de resumo. Percorra o restante do assistente e concluir a configuração.

Para habilitar a autenticação on-behalf-of, precisamos garantir que o AD FS retorna um token de acesso com escopo user_impersonation ao cliente. Modifique a emissão de declarações para ToDoListServiceWebApi incluir três regras personalizadas a seguir:

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue open id scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "openid");

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**Adicionando ToDoListService como um cliente no grupo de aplicativos**

Neste estágio, é preciso criar uma entrada adicional no AD FS para o servidor Web App atuar como um cliente e não apenas como um recurso. Abra o grupo de aplicativos que você acabou de criar e clique em Adicionar aplicativo.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

Você verá a página "Adicionar um novo aplicativo para MySampleGroup". Nessa página, selecione "Aplicativo ou site do servidor" como o aplicativo autônomo

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

Clique em Avançar e você verá a página para fornecer detalhes do aplicativo. Forneça um nome adequado para a entrada de configuração na seção de nome. Certifique-se de que o identificador de cliente é o mesmo que o identificador para o ToDoListServiceWebAPI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

Clique em Avançar e você verá a página para configurar as credenciais do aplicativo. Clique em "Gerar um segredo compartilhado". Você verá um segredo que é gerado automaticamente. Copie o segredo em algum local, pois isso será necessário enquanto configuramos ToDoListService no visual studio.


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

Clique em Avançar e concluir o assistente.

### <a name="modifying-the-todolistclient-code"></a>Modificar o código de ToDoListClient

#### <a name="modify-the-application-config"></a>Modificar a configuração de aplicativo

Vá até que você está projeto ToDoListClient na solução OnBehalfOf-WebAPI-DotNet. Abra o arquivo App. config e fazer as seguintes modificações

* A entrada de chave ida: locatários de comentário
* Para o RedirectURI do ida: insira o URI arbitrário que você forneceu ao configurar o MySampleGroup_ClientApplication no AD FS.
* Para a chave ida: ClientID, forneça o cliente para o identificador de ID que o AD FS deu ao configurar o MySampleGroup_ClientApplication.
* Para a ida: ToDoListResourceID forneça a ID de recurso que você forneceu ao configurar o ToDoListServiceWebApi no AD FS
* A chave ida: AADInstance de comentário
* Para a ida: ToDoListBaseAddress, insira a ID de recurso do ToDoListServiceWebApi. Isso será usado ao chamar o ToDoList WebAPI.
* Adicionar uma chave ida: autoridade e forneça o valor como o URI para o AD FS.

Sua **appSettings** em App. config deve ser semelhante a esta:

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

Comente a linha lendo as informações de locatário a configuração de aplicativo

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

Altere o valor da autoridade de cadeia de caracteres

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

Alterar o código para ler os valores corretos de ToDoListResourceId e ToDoListBaseAddress

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

Na função MainWindow() altere a inicialização de authcontext como:

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>Adicione o recurso de back-end

Para concluir o fluxo on-behalf-of, você precisa criar um recurso de back-end que irá acessar ToDoListService em nome de usuário autenticado. A escolha do recurso de back-end pode variar de acordo com o requisito, mas para fins de neste exemplo, você pode criar uma API da Web básico.

* Clique com o botão direito na solução 'OnBehalfOf-WebAPI-DotNet' no Gerenciador de soluções e selecione Adicionar -> Novo projeto
* Escolha o modelo de aplicativo Web ASP.NET

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* Na próxima prompt, clique em 'Alterar autenticação'
* Selecione 'Trabalho e contas de estudante' e na lista suspensa à direita, selecione 'Local'
* Insira o caminho federationmetadata para sua implantação do AD FS e forneça um URI do aplicativo (forneça qualquer URI por enquanto, e você alterá-lo mais tarde) e clique em Okey para adicionar o projeto à solução.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* Clique com botão direito em controladores no Gerenciador de soluções sob o novo projeto criado. Selecione Adicionar -> controlador
* Em seleção de modelo, selecione ' Web API 2 Controller - vazio' e clique em Okey.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* Nomeie o controlador apropriado

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

Esse código simplesmente retornará a cadeia de caracteres quando alguém coloca uma solicitação Get para o WebAPI WebAPIOBO

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>Adicionar o novo back-end WebAPI para o AD FS

Abra o grupo de aplicativos MySampleGroup. Clique em Adicionar aplicativo e selecionar o modelo de API da Web e clique em Avançar.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

Na página Configurar API da Web, forneça um nome apropriado para a entrada da API Web e o identificador. O identificador deve ser o valor da URL do SSL do projeto WebAPIOBO no visual studio (semelhante ao que fizemos para BackendWebAPIAdfsAdd).

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

Prossiga com o restante do assistente mesmo como quando configuramos o ToDoListService WebAPI. No final, seu grupo de aplicativos deve ser semelhante a:

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>Modificar o código de ToDoListService

#### <a name="modifying-the-application-config"></a>Modificando a configuração de aplicativo

* Abra o arquivo Web. config
* Modifique as seguintes chaves

| Chave | Valor |
|:-----|:-------|
|ida: público-alvo| ID do ToDoListService conforme fornecido para o AD FS ao configurar o ToDoListService WebAPI, por exemplo, https://localhost:44321/|
|ida:ClientID| ID do ToDoListService conforme fornecido para o AD FS ao configurar o ToDoListService WebAPI, por exemplo, https://localhost:44321/ </br>**É muito importante que a ida: público-alvo e ida: ClientID correspondem uns aos outros**|
|ida:ClientSecret| Esse é o segredo gerada pelo AD FS ao configurar o cliente ToDoListService no AD FS|
|ida:ADFSMetadata| Esta é a URL para os metadados do AD FS, para, por exemplo https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml|
|ida:OBOWebAPIBase| Esse é o endereço base que usaremos para chamar o back-end de API, por exemplo https://localhost:44300|
|ida:Authority| Esta é a URL para o serviço do AD FS, exemplo https://fs.anandmsft.com/adfs/|


As chaves de todos os outro ida: XXXXXXX na **appsettings** nó pode ser comentada ou excluída

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>Alterar a autenticação do Azure AD para o AD FS

* Abra o arquivo Startup.Auth.cs
* Remover o código a seguir

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

Adicione referência a Extensions. Modificar os membros de classe, substituindo o código a seguir

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

**Modificar a declaração usada para nome**

Do AD FS estamos disponibilizando a declaração Nmae, mas não estamos emitindo declaração NameIdentifier. O exemplo usa NameIdentifier para exclusivamente a chave em itens de tarefas. Para simplificar, você pode remover com segurança o NameIdentifier com a declaração de nome no código. Localizar e substituir todas as ocorrências de NameIdentifier com o nome.

**Modificar a rotina de postagem e CallGraphAPIOnBehalfOfUser()**

Copie e cole o código abaixo no ToDoListController.cs e substitua o código para Post e CallGraphAPIOnBehalfOfUser

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
                result = await authContext.AcquireTokenAsync(...);
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

## <a name="running-the-solution"></a>Executar a solução


Por padrão, o visual studio está configurado para executar um projeto quando você atinge a depuração para executar.

* Clique com botão direito na solução e selecione Propriedades.
* Na página de propriedades selecione projetos de inicialização de vários e altere a ação Iniciar para todas as três entradas.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

Pressione F5 e execute a solução

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

Clique no botão de entrada. Você será solicitado a entrar usando o AD FS

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

Depois de entrar, adicione um item de tarefas na lista. Nos bastidores vamos fazer uma operação Post para o que mais fará um Post para a API da web WebAPIOBO ToDoListService.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

Na operação com êxito, você verá que o item foi adicionado à lista com a mensagem adicional da API da Web que foi acessado por meio de fluxo de autenticação OBO back-end.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

Você também pode ver os rastreamentos detalhados no Fiddler. Inicie o Fiddler e habilitar a descriptografia de HTTPS. Você pode ver o que podemos fazer duas solicitações para o ponto de extremidade /adfs/oautincludes.
Na primeira interação, podemos apresentar o código de acesso para o ponto de extremidade de token e obter token para um tipo de acesso https://localhost:44321/ ![OBO do AD FS](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

A segunda interação com o ponto de extremidade de token, você pode ver que temos **requested_token_use** definido como **on_behalf_of** e estamos usando o token de acesso obtido para o serviço web de camada intermediária, ou seja, https://localhost:44321/ como a declaração para obter o token on-behalf-of.
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  
