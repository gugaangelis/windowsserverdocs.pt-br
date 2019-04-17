---
title: "Crie um aplicativo da web de página única usando OAuth e ADAL.JS com o AD FS 2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 7b3d48e1e38baffeb84b1f236efb43cfda5048c0
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016"></a>Crie um aplicativo da web de página única usando OAuth e ADAL.JS com o AD FS 2016

>Aplica-se a: Windows Server 2016

Este passo a passo fornece instruções para se autenticar no AD FS usando ADAL para JavaScript protegendo uma AngularJS com base em aplicativo de página única, implementado com um back-end de API da Web ASP.NET.

>Aviso: O exemplo que você pode criar aqui é apenas para fins educacionais. Estas instruções são para a implementação mais simples, mais mínima possível expor os elementos necessários do modelo. O exemplo não pode incluir todos os aspectos do tratamento de erros e outros se relacionam a funcionalidade.

>[!NOTE]
>Este passo a passo é aplicável **somente** para AD FS Server 2016 e superior 

## <a name="overview"></a>Visão geral
Neste exemplo estamos criando um fluxo de autenticação em que um cliente de aplicativo única página estará autenticando contra AD FS para proteger o acesso aos recursos WebAPI no back-end. Abaixo está o fluxo geral de autenticação


![AD FS autorização](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Ao usar um aplicativo de página única, o usuário navega para um local de partida de onde a partir de página e um conjunto de arquivos JavaScript e modos de exibição HTML são carregados. Você precisa configurar o Active Directory autenticação biblioteca (ADAL) para saber as informações cruciais sobre seu aplicativo, ou seja, a instância do AD FS, ID do cliente, para que ele pode direcionar a autenticação para o AD FS.

Se ADAL vê um gatilho para autenticação, ele usa as informações fornecidas pelo aplicativo e direciona a autenticação ao seu STS do AD FS.  O aplicativo de página única, que é registrado como um cliente no AD FS público, é configurado automaticamente para o fluxo de concessão implícita. A solicitação de autorização resulta em um token de ID é retornado para o aplicativo por meio de um #fragment. Outras chamadas para o back-end WebAPI terá esse token ID como o token bearer no cabeçalho para acessar o WebAPI.

## <a name="setting-up-the-development-box"></a>Configurando a caixa de desenvolvimento
Este passo a passo usa o Visual Studio 2015. O projeto usa JS ADAL biblioteca. Para saber mais sobre ADAL leia [Active Directory autenticação biblioteca .NET.](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>Configurando o ambiente
Neste passo a passo usaremos uma instalação básica do:

1.  DC: Controlador de domínio para o domínio em que será hospedado AD FS
2.  FS AD: O AD FS Server para o domínio
3.  : Desenvolvimento máquina onde podemos Visual Studio instalado e será ser desenvolvendo nossa amostra

Você pode usar se você quiser, apenas dois computadores. Um para DC/AD FS e outro para desenvolver a amostra.

Como configurar o controlador de domínio e o AD FS está além do escopo deste artigo. Para obter informações, consulte implantação adicionais:

- [Implantação do AD DS](../../ad-ds/deploy/AD-DS-Deployment.md) 
- [AD FS implantação](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Clone ou baixar esse repositório
Usaremos o aplicativo de exemplo criado para integração do Azure AD em um aplicativo de página única AngularJS e modificá-la para proteger em vez disso, o recurso de back-end usando o AD FS.

De um shell ou uma linha de comando:

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>Sobre o código
Os arquivos de chave que contém a lógica de autenticação são as seguintes:

**App.js** - injeta a dependência de módulo ADAL, fornece os valores de configuração do aplicativo usados pelo ADAL para dirigir interações de protocolo com AAD e indica que rotas não devem ser acessadas sem autenticação anterior.

**index** -contém uma referência ao adal.js

**HomeController.js**-mostra como tirar proveito dos métodos login () e logOut() em ADAL.

**UserDataController.js** -mostra como extrair informações do usuário do id_token em cache.

**Startup.Auth.cs** -contém a configuração de WebAPI usar o serviço de Federação do Active Directory para autenticação bearer.

## <a name="registering-the-public-client-in-ad-fs"></a>Registrar o cliente público no AD FS
No exemplo, o WebAPI está configurado para ouvir quando https://localhost:44326 /. O grupo de aplicativos **navegador da Web acessando um aplicativo web** pode ser usado para configurar o aplicativo de fluxo de concessão implícita.

1. Abra o console de gerenciamento do AD FS e clique em **adicionar grupo de aplicativos**. No **Assistente para adicionar grupo de aplicativos** insira o nome do aplicativo, descrição e selecione o **navegador da Web acessando um aplicativo web** modelo do **aplicativos cliente-servidor** seção conforme mostrado abaixo
    <br>![Criar novo grupo de aplicativos](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. Na próxima página **aplicativo nativo**, fornecer o identificador de cliente do aplicativo e o URI de redirecionamento, conforme mostrado abaixo
    <br>![Criar novo grupo de aplicativos](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. Na próxima página **Aplicar política de controle de acesso** deixar as permissões como *permitir todos*

4. A página de resumo deve ser parecida com abaixo
    <br>![Criar novo grupo de aplicativos](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Clique em **próxima** para concluir a adição do grupo de aplicativos e feche o assistente.

## <a name="modifying-the-sample"></a>Modificar o exemplo
Configurar ADAL JS

Abrir o **app.js** do arquivo e altere o **adalProvider.init** definição para:

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
        );

|Configuração|Descrição
|--------|--------
|instância|Sua URL STS, por exemplo, https://fs.contoso.com/
|locatário|Mantê-los como 'adfs'
|clientID|Essa é a ID de cliente que você especificou ao configurar o cliente público para seu aplicativo de página única

## <a name="configure-webapi-to-use-ad-fs"></a>Configurar WebAPI para usar o AD FS
Abrir o **Startup.Auth.cs** no exemplo do arquivo e adicione o seguinte no início: 

    using System.IdentityModel.Tokens;

Remova:

                app.UseWindowsAzureActiveDirectoryBearerAuthentication(
    new WindowsAzureActiveDirectoryBearerAuthenticationOptions
    {
    Audience = ConfigurationManager.AppSettings["ida:Audience"],
    Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
    });

e adicione:

    app.UseActiveDirectoryFederationServicesBearerAuthentication(
    new ActiveDirectoryFederationServicesBearerAuthenticationOptions
    {
    MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
    TokenValidationParameters = new TokenValidationParameters()
    {
    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"],
    ValidIssuer = ConfigurationManager.AppSettings["ida:Issuer"]
    }
    }
    );

|Parâmetro|Descrição
|--------|--------
|ValidAudience|Isso configura o valor de 'público' que será verificada no token
|ValidIssuer|Isso configura o valor de ' emissor que será verificada no token
|MetadataEndpoint|Isso apontará para as informações de metadados de seu STS

## <a name="add-application-configuration-for-ad-fs"></a>Adicione uma configuração de aplicativo para o AD FS
Altere o appsettings abaixo:

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>Executando a solução
Limpe a solução, recompile a solução e executá-lo. Se você quiser ver rastreamentos detalhados, inicie o Fiddler e habilite a descriptografia HTTPS.

O navegador carregará o SPA e você verá a tela a seguir:

![Registrar o cliente](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Clique em Login.  A lista ToDo disparará o fluxo de autenticação e JS ADAL direcionará a autenticação para AD FS

![Logon](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

No Fiddler, você pode ver o token está sendo retornado como parte da URL no fragmento de #.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

Você poderá chamar agora o API para adicionar itens de lista de tarefas pendentes para o usuário conectado no back-end:

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>Próximas etapas
[AD FS desenvolvimento](../../ad-fs/AD-FS-Development.md)  
