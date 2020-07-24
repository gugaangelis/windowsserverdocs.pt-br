---
title: Criar um aplicativo Web de página única usando OAuth e ADAL.JS com o AD FS 2016 ou posterior
description: Uma explicação que fornece instruções para autenticação no AD FS usando a ADAL para JavaScript protegendo um aplicativo de página única baseado em AngularJS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/13/2018
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.openlocfilehash: 1bd5d95739bc1c975f5f0c4d7efb8dc6f91e0412
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954398"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016-or-later"></a>Criar um aplicativo Web de página única usando OAuth e ADAL.JS com o AD FS 2016 ou posterior

Este tutorial fornece instruções para autenticação no AD FS usando a ADAL para JavaScript, protegendo um aplicativo de página única baseado em AngularJS, implementado com um back-end ASP.NET Web API.

Nesse cenário, quando o usuário faz logon, o front-end de JavaScript usa a [Biblioteca de Autenticação do Active Directory para JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) e a concessão de autorização implícita para obter um token de ID (id_token) do Azure AD. O token é armazenado em cache e o cliente o anexa à solicitação como o token de portador ao fazer chamadas paro o back-end de sua API da Web, que é protegido usando o middleware OWIN.

>[!IMPORTANT]
>O exemplo que você pode criar aqui é apenas para fins educacionais. Essas instruções são para a implementação mais simples e mínima possível para expor os elementos necessários do modelo. O exemplo pode não incluir todos os aspectos do tratamento de erros e outras funcionalidades relacionadas.

>[!NOTE]
>Este tutorial é aplicável **somente** ao AD FS Server 2016 e posterior 

## <a name="overview"></a>Visão geral
Neste exemplo, criaremos um fluxo de autenticação em que um cliente de aplicativo de página única será autenticado AD FS para proteger o acesso aos recursos do WebAPI no back-end. Abaixo está o fluxo de autenticação geral


![Autorização de AD FS](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Ao usar um aplicativo de página única, o usuário navega para um local inicial, de onde a página inicial e uma coleção de arquivos JavaScript e exibições HTML são carregadas. Você precisa configurar o Biblioteca de Autenticação do Active Directory (ADAL) para saber as informações críticas sobre seu aplicativo, ou seja, a instância de AD FS, ID do cliente, para que ele possa direcionar a autenticação para seu AD FS.

Se a ADAL vir um gatilho para autenticação, ela usará as informações fornecidas pelo aplicativo e direcionará a autenticação para seu AD FS STS.  O aplicativo de página única, que é registrado como um cliente público no AD FS, é configurado automaticamente para o fluxo de concessão implícita. A solicitação de autorização resulta em um token de ID que é retornado para o aplicativo por meio de um #fragment. Chamadas adicionais para o WebAPI de back-end terão esse token de ID como o token de portador no cabeçalho para obter acesso ao WebAPI.

## <a name="setting-up-the-development-box"></a>Configurando a caixa de desenvolvimento
O passo a passo usa o Visual Studio 2015. O projeto usa a biblioteca do ADAL JS. Para saber mais sobre a ADAL, leia [biblioteca de autenticação do Active Directory .net.](/dotnet/api/microsoft.identitymodel.clients.activedirectory?view=azure-dotnet)

## <a name="setting-up-the-environment"></a>Configurando o ambiente
Neste tutorial, usaremos uma configuração básica de:

1.    DC: controlador de domínio para o domínio no qual AD FS será hospedado
2.    Servidor de AD FS: o servidor de AD FS para o domínio
3.    Máquina de desenvolvimento: computador onde o Visual Studio foi instalado e desenvolverei nosso exemplo

Você pode, se desejar, usar apenas duas máquinas. Um para DC/AD FS e o outro para desenvolver o exemplo.

Como configurar o controlador de domínio e AD FS está além do escopo deste artigo. Para obter informações adicionais de implantação, consulte:

- [Implantação do AD DS](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Implantação do AD FS](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Clonar ou baixar este repositório
Usaremos o aplicativo de exemplo criado para integrar o Azure AD em um aplicativo de página única do AngularJS e modificá-lo para proteger o recurso de back-end usando AD FS.

No shell ou na linha de comando:

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>Sobre o código
Os arquivos de chave que contêm a lógica de autenticação são os seguintes:

**App.js** -injeta a dependência do módulo Adal, fornece os valores de configuração de aplicativo usados pela Adal para conduzir interações de protocolo com o AAD e indica quais rotas não devem ser acessadas sem autenticação anterior.

**index.html** -contém uma referência a adal.js

**HomeController.js**-mostra como aproveitar os métodos de logon () e logOut () no Adal.

**UserDataController.js** -mostra como extrair informações do usuário do id_token em cache.

**Startup.auth.cs** -contém a configuração para o WebAPI usar Active Directory serviço de Federação para autenticação de portador.

## <a name="registering-the-public-client-in-ad-fs"></a>Registrando o cliente público no AD FS
No exemplo, o WebAPI está configurado para escutar em https://localhost:44326/ . O navegador da Web do grupo de aplicativos que **acessa um aplicativo Web** pode ser usado para configurar o aplicativo de fluxo de concessão implícito.

1. Abra o console de gerenciamento do AD FS e clique em **Adicionar grupo de aplicativos**. No **Assistente para Adicionar grupo de aplicativos** , insira o nome do aplicativo, a descrição e selecione o **navegador da Web acessando um modelo de aplicativo Web** na seção **aplicativos cliente-servidor** , como mostrado abaixo

    ![Criar novo grupo de aplicativos](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. No **aplicativo nativo**da próxima página, forneça o identificador do cliente do aplicativo e o URI de redirecionamento, conforme mostrado abaixo

    ![Criar novo grupo de aplicativos](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. Na próxima página, **aplique a política de controle de acesso** e deixe as permissões como *permitir todos*

4. A página de resumo deve ser semelhante à seguinte

    ![Criar novo grupo de aplicativos](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Clique em **Avançar** para concluir a adição do grupo de aplicativos e feche o assistente.

## <a name="modifying-the-sample"></a>Modificando o exemplo
Configurar ADAL JS

Abra o arquivo **app.js** e altere a definição de **tadalProvider.ini** para:

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
    );

|Configuração|Descrição|
|--------|--------|
|instance|A URL do STS, por exemplo,https://fs.contoso.com/|
|locatário|Mantenha-o como ' ADFS '|
|clientID|Essa é a ID do cliente que você especificou ao configurar o cliente público para seu aplicativo de página única|

## <a name="configure-webapi-to-use-ad-fs"></a>Configurar o WebAPI para usar AD FS
Abra o arquivo **Startup.auth.cs** no exemplo e adicione o seguinte no início:

    using System.IdentityModel.Tokens;

exclu

    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        }
    );

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

|Parâmetro|Descrição|
|--------|--------|
|ValidAudience|Isso configura o valor de ' Audience ' que será verificado no token|
|ValidIssuer|Isso configura o valor de "emissor que será verificado em relação ao token|
|MetadataEndpoint|Isso aponta para as informações de metadados de seu STS|

## <a name="add-application-configuration-for-ad-fs"></a>Adicionar configuração de aplicativo para AD FS
Altere os appSettings da seguinte maneira:
```xml
<appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
</appSettings>
```

## <a name="running-the-solution"></a>Executando a solução
Limpe a solução, recompile a solução e execute-a. Se você quiser ver rastreamentos detalhados, inicie o Fiddler e habilite a descriptografia de HTTPS.

O navegador (use o navegador Chrome) carregará o SPA e você verá a seguinte tela:

![Registrar o cliente](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Clique em logon.  A lista de tarefas disparará o fluxo de autenticação e a ADAL JS direcionará a autenticação para AD FS

![Logon](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

No Fiddler, você pode ver o token que está sendo retornado como parte da URL no fragmento #.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

Agora, você poderá chamar a API de back-end para adicionar itens de lista de tarefas para o usuário conectado:

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  
