---
title: Crie um aplicativo web de página única usando ADAL e OAuth. JS com o AD FS 2016 ou posterior
description: Um passo a passo que fornece instruções para se autenticar no AD FS usando a ADAL para JavaScript protegendo um AngularJS com base em aplicativo de página única
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 24a9caba7a2745973d7c69c3bd7bc42717e7a06c
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266689"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016-or-later"></a>Crie um aplicativo web de página única usando ADAL e OAuth. JS com o AD FS 2016 ou posterior

Este passo a passo fornece instruções para se autenticar no AD FS usando o ADAL para JavaScript protegendo um AngularJS baseado em aplicativo de página única implementado com um back-end de API Web do ASP.NET.

Nesse cenário, quando o usuário faz logon, o JavaScript front-end usa [Active Directory Authentication Library para JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) e a concessão de autorização implícita para obter um token de ID (id_token) do Azure AD. O token é armazenado em cache e o cliente o anexa à solicitação de token de portador ao fazer chamadas à API da Web de back-end, que é protegido usando o middleware OWIN.

>AVISO: O exemplo que você pode criar aqui é apenas para fins educacionais. Essas instruções são para a implementação mais simples e mais concisa possível expor os elementos necessários do modelo. O exemplo pode não incluir todos os aspectos de tratamento de erros e outros relacionados a funcionalidade.

>[!NOTE]
>Este passo a passo é aplicável **apenas** ao servidor do AD FS 2016 e posterior 

## <a name="overview"></a>Visão geral
Neste exemplo estamos criando um fluxo de autenticação em que um cliente de aplicativo de página única estará autenticando com o AD FS para proteger o acesso aos recursos da API Web no back-end. Abaixo está o fluxo geral de autenticação


![Autorização do AD FS](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Ao usar um aplicativo de página única, o usuário navega para um local de início, de onde iniciar a página e uma coleção de arquivos JavaScript e modos de exibição HTML são carregados. Você precisa configurar o Active Directory autenticação ADAL (biblioteca) para saber informações importantes sobre seu aplicativo, ou seja, a instância do AD FS, ID do cliente, para que ele pode direcionar a autenticação do AD FS.

Se um gatilho para autenticação ADAL, ele usa as informações fornecidas pelo aplicativo e direciona a autenticação do STS do AD FS.  O aplicativo de página única, que é registrado como um cliente público no AD FS, é configurado automaticamente para o fluxo de concessão implícita. A solicitação de autorização resulta em um token de ID que é retornado para o aplicativo por meio de um #fragment. Mais chamadas para o back-end WebAPI realizar esse token de ID como o token de portador no cabeçalho para acessar a API da Web.

## <a name="setting-up-the-development-box"></a>Como configurar a caixa de desenvolvimento
Este passo a passo usa o Visual Studio 2015. O projeto usa a biblioteca ADAL JS. Para saber mais sobre a ADAL, leia [.NET da biblioteca autenticação do Active Directory.](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>Configurando o ambiente
Para este passo a passo vamos usar uma configuração básica de:

1.  DC: Controlador de domínio para o domínio no qual o AD FS será hospedado.
2.  Servidor do AD FS: O servidor do AD FS para o domínio
3.  Máquina de desenvolvimento: Computador em que podemos ter instalado o Visual Studio e desenvolverá nossa amostra

Você pode usar se desejar, somente duas máquinas. Um para DC/AD FS e o outro para o desenvolvimento de exemplo.

Como configurar o controlador de domínio e o AD FS está além do escopo deste artigo. Para obter informações adicionais de implantação consulte:

- [Implantação do AD DS](../../ad-ds/deploy/AD-DS-Deployment.md) 
- [Implantação do AD FS](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Clonar ou baixar este repositório
Usaremos o aplicativo de exemplo criado para integração do Azure AD em um aplicativo de página única AngularJS e modificá-lo para proteger em vez disso, o recurso de back-end por meio do AD FS.

No shell ou na linha de comando:

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>Sobre o código
Os arquivos de chave que contém a lógica de autenticação são as seguintes:

**App. js** - injeta a dependência do módulo ADAL, fornece os valores de configuração de aplicativo usados pelo ADAL para impulsionar a interações de protocolo com o AAD e indica quais rotas não devem ser acessadas sem autenticação anterior.

**index. HTML** -contém uma referência à adal. js

**HomeController.js**-mostra como tirar proveito dos métodos de login () e logOut() em ADAL.

**UserDataController.js** -mostra como extrair informações do usuário do id_token em cache.

**Startup.Auth.cs** -contém a configuração para a API da Web usar o serviço de Federação do Active Directory para autenticação do portador.

## <a name="registering-the-public-client-in-ad-fs"></a>Registrando o cliente público no AD FS
No exemplo, a API da Web está configurado para escutar em https://localhost:44326/. O grupo de aplicativos **navegador da Web acessando um aplicativo web** pode ser usado para configurar o aplicativo de fluxo de concessão implícita.

1. Abra o console de gerenciamento do AD FS e clique em **adicionar grupo de aplicativos**. No **Assistente para adicionar grupo de aplicativos** insira o nome do aplicativo, descrição e selecione o **navegador da Web acessando um aplicativo web** modelo a partir o **cliente-servidor aplicativos** seção conforme mostrado abaixo

    ![Criar novo grupo de aplicativos](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. Na página seguinte **aplicativo nativo**, forneça o identificador de cliente do aplicativo e URI de redirecionamento, conforme mostrado abaixo

    ![Criar novo grupo de aplicativos](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. Na página seguinte **Aplicar política de controle de acesso** deixar as permissões como *permitir todos*

4. A página de resumida deve ser semelhante ao mostrado abaixo

    ![Criar novo grupo de aplicativos](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Clique em **próxima** para concluir a adição do grupo de aplicativos e fechar o assistente.

## <a name="modifying-the-sample"></a>Modificando o exemplo
Configurar a ADAL JS

Abra o **App. js** do arquivo e altere o **adalProvider.init** definição para:

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
|instância|A URL do STS, por exemplo, https://fs.contoso.com/
|tenant|Mantenha-o como 'adfs'
|clientID|Esta é a ID de cliente que você especificou ao configurar o cliente público para seu aplicativo de página única

## <a name="configure-webapi-to-use-ad-fs"></a>Configurar API da Web para usar o AD FS
Abra o **Startup.Auth.cs** no exemplo de arquivo e adicione o seguinte no início: 

    using System.IdentityModel.Tokens;

remove:

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
|ValidAudience|Isso configura o valor de 'público' que serão verificados em relação a no token
|ValidIssuer|Isso configura o valor de ' emissor que será verificado em relação a no token
|MetadataEndpoint|Isso aponta para as informações de metadados de seu STS

## <a name="add-application-configuration-for-ad-fs"></a>Adicionar configuração de aplicativo do AD FS
Altere o appsettings, conforme mostrado a seguir:

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>Executar a solução
Limpa a solução, recompile a solução e executá-lo. Se você quiser ver rastreamentos detalhados, inicie o Fiddler e habilitar a descriptografia de HTTPS.

O navegador (use o navegador Chrome) carregará o SPA e você verá a tela a seguir:

![Registrar o cliente](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Clique no logon.  Lista de tarefas irá disparar o fluxo de autenticação e a ADAL JS direcionará a autenticação do AD FS

![logon](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

No Fiddler, você pode ver o token seja retornado como parte da URL no fragmento #.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

Você poderá chamar o back-end de API para adicionar itens de lista de tarefas pendentes para o usuário conectado:

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  
