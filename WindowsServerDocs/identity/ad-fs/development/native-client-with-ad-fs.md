---
title: Criar um aplicativo cliente nativo usando clientes OAuth públicos com o AD FS 2016 ou posterior
description: Uma explicação que fornece instruções sobre como criar um aplicativo cliente nativo usando clientes públicos OAuth e o AD FS 2016 ou posterior
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 2eb2d0a3cfa6763d308bbd73f1ccf50795b06e77
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866339"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016-or-later"></a>Criar um aplicativo cliente nativo usando clientes OAuth públicos com o AD FS 2016 ou posterior

## <a name="overview"></a>Visão geral

Este artigo mostra como criar um aplicativo nativo que interage com uma API da Web protegida pelo AD FS 2016 ou posterior.

1. O aplicativo .net TodoListClient WPF usa o Biblioteca de Autenticação do Active Directory (ADAL) para obter um token de acesso JWT do Azure Active Directory (AD do Azure) por meio do protocolo OAuth 2,0
2. O token de acesso é usado como um token de portador para autenticar o usuário ao chamar o ponto de extremidade/ToDoList da API Web TodoListService.
 Usaremos o exemplo de aplicativo para o AD do Azure aqui e, em seguida, o modificaremos para AD FS 2016 ou posterior.

![Overivew do aplicativo](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>Pré-requisitos
Veja a seguir uma lista de pré-requisitos que são necessários antes de concluir este documento. Este documento pressupõe que AD FS foi instalado e um farm de AD FS foi criado.

* Ferramentas de cliente do GitHub
* AD FS no Windows Server 2016 ou posterior
* Visual Studio 2013 ou posterior

## <a name="creating-the-sample-walkthrough"></a>Criando o exemplo de explicação

### <a name="create-the-application-group-in-ad-fs"></a>Criar o grupo de aplicativos no AD FS

1. Em gerenciamento de AD FS, clique com o botão direito do mouse em **grupos de aplicativos** e selecione **Adicionar grupo de aplicativos**.

2. No assistente de grupo de aplicativos, para o nome, insira qualquer nome que você preferir, por exemplo, NativeToDoListAppGroup. Selecione o **aplicativo nativo acessando um modelo de API Web** . Clique em **Avançar**.
 ![Adicionar grupo de aplicativos](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. Na página **aplicativo nativo** , observe o identificador gerado pelo AD FS. Essa é a ID com a qual AD FS reconhecerá o aplicativo cliente público. Copie o valor do **identificador de cliente** . Ele será usado posteriormente como o valor de **ida: ClientID** no código do aplicativo. Se desejar, você pode fornecer qualquer identificador personalizado aqui. O URI de redirecionamento é qualquer valor arbitrário, https://ToDoListClient exemplo, colocar ![ aplicativo nativo](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. Na página **Configurar API Web** , defina o valor do identificador para a API Web. Para este exemplo, esse deve ser o valor da **URL SSL** em que o aplicativo Web deve estar em execução. Você pode obter esse valor clicando nas propriedades do projeto TooListServer na solução. Isso será usado posteriormente como o valor **todo: TodoListResourceId** no arquivo **app. config** do aplicativo cliente nativo e também como **todo: TodoListBaseAddress**.
![API Web](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. Siga as permissões **aplicar política de controle de acesso** e **Configurar aplicativo** com os valores padrão em vigor. A página resumo deve ter a seguinte aparência.
![Resumo](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)

Clique em avançar e conclua o assistente.

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>Adicionar a declaração NameIdentifier à lista de declarações emitidas
O aplicativo de demonstração usa o valor na declaração NameIdentifier em vários locais. Ao contrário do Azure AD, o AD FS não emite uma declaração NameIdentifier por padrão. Portanto, precisamos adicionar uma regra de declaração para emitir a declaração NameIdentifier para que o aplicativo possa usar o valor correto. Neste exemplo, o nome fornecido do usuário é emitido como o valor de NameIdentifier para o usuário no token.
Para configurar a regra de declaração, abra o grupo de aplicativos recém-criado e clique duas vezes na API Web. Selecione a guia regras de transformação de emissão e clique no botão Adicionar regra. No tipo de regra de declaração, escolha regra de declaração personalizada e, em seguida, adicione a regra de declaração, conforme mostrado abaixo.

```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"), query = ";givenName;{0}", param = c.Value);
```

![Regra de declaração NameIdentifier](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>Modificar o código do aplicativo

Esta seção discute como baixar a API Web de exemplo e modificá-la no Visual Studio.   Usaremos o exemplo do Azure AD [aqui](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop).  

Para baixar o projeto de exemplo, use git bash e digite o seguinte:  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-native-desktop  
```  

#### <a name="modify-todolistclient"></a>Modificar ToDoListClient

Esse projeto na solução representa o aplicativo cliente nativo. Precisamos garantir que o aplicativo cliente saiba:

1. Onde ir para que o usuário seja autenticado quando necessário?
2. Qual é a ID que o cliente precisa fornecer à autoridade de autenticação (AD FS)?
3. Qual é a ID do recurso para o qual estamos solicitando o token de acesso?
4. Qual é o endereço base da API Web?

As alterações de código a seguir são necessárias para obter as informações acima para o aplicativo cliente nativo.

**App. config**

* Adicione a chave **ida: Authority** com o valor que descreve o serviço AD FS. Por exemplo, https://fs.contoso.com/adfs/
* Modifique a chave **ida: ClientID** com o valor do **identificador de cliente** na página do **aplicativo nativo** durante a criação do grupo de aplicativos no AD FS. Por exemplo, 3f07368b-6efd-4f50-A330-d93853f4c855
* Modifique **todo: todo: TodoListResourceId** com o valor do **identificador** na página **Configurar API da Web** durante a criação do grupo de aplicativos no AD FS. Por exemplo, https://localhost:44321/
* Modifique **todo: TodoListBaseAddress** com o valor de **identificador** na página **Configurar API da Web** durante a criação do grupo de aplicativos no AD FS. Por exemplo, https://localhost:44321/
* Defina o valor de **ida: RedirectUri** com o valor do **URI de redirecionamento** na página do **aplicativo nativo** durante a criação do grupo de aplicativos em AD FS. Por exemplo, https://ToDoListClient
* Para facilitar a leitura, você pode remover/comentar a chave para **ida: Tenant** e **ida: AADInstance**.

  ![Configuração do aplicativo](media/native-client-with-ad-fs-2016/app_configfile.PNG)


**MainWindow.xaml.cs**

* Comente a linha para aadInstance como abaixo

        // private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];

* Adicione o valor da autoridade como abaixo

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* Exclua a linha para criar o valor de **autoridade** de aadInstance e locatário

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* Na função **MainWindow**, altere a instanciação authContext para

        authContext = new AuthenticationContext(authority,false);

    A ADAL não dá suporte à validação de AD FS como autoridade e, portanto, temos que passar um sinalizador de valor falso para o parâmetro validateAuthority.

  ![Janela principal](media/native-client-with-ad-fs-2016/mainwindow.PNG)

#### <a name="modify-todolistservice"></a>Modificar TodoListService
Dois arquivos precisam de alterações neste projeto – Web. config e Startup.Auth.cs. As alterações de Web. config são necessárias para obter os valores corretos dos parâmetros. Alterações de Startup.Auth.cs são necessárias para definir o WebAPI para autenticar em AD FS em vez do Azure AD.

**Web.config**

* Comente a chave **ida: Tenant** , pois não precisamos dela
* Adicione a chave para **ida: Authority** com o valor que indica o FQDN do serviço de Federação, por exemplo, https://fs.contoso.com/adfs/
* Modificar chave **ida: Audience** com o valor do identificador da API Web especificado na página **Configurar API da Web** durante a adição do grupo de aplicativos no AD FS.
* Adicione a chave **ida: AdfsMetadataEndpoint** com o valor correspondente à URL de metadados de Federação do serviço de AD FS, por ex: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

![Configuração da Web](media/native-client-with-ad-fs-2016/webconfig.PNG)


**Startup.Auth.cs**

Modifique a função ConfigureAuth como abaixo

    public void ConfigureAuth(IAppBuilder app)
    {
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
    }

Basicamente, estamos Configurando a autenticação para usar AD FS e fornecer mais informações sobre os metadados de AD FS, e para validar o token, a declaração de audiência deve ser o valor esperado pela API Web.
Executando o aplicativo

1. Na solução NativeClient-DotNet, clique com o botão direito e vá para propriedades. Altere o projeto de inicialização conforme mostrado abaixo para vários projetos de inicialização e defina TodoListClient e TodoListService como Start.
![Propriedades da solução](media/native-client-with-ad-fs-2016/solutionproperties.png)

2.  Pressione o botão F5 ou selecione Depurar > continuar na barra de menus. Isso abrirá o aplicativo nativo e o WebAPI. Clique no botão entrar no aplicativo nativo e ele exibirá um logon interativo do AD AL e redirecionará para o serviço de AD FS. Insira as credenciais de um usuário válido.
![Entrar](media/native-client-with-ad-fs-2016/sign-in.png)

Nesta etapa, o aplicativo nativo Redirecionado para AD FS e obteve um token de ID e um token de acesso para a API Web

3. Insira um item de tarefas pendentes na caixa de texto e clique em Adicionar item. Nesta etapa, o aplicativo chega à API Web para adicionar o item de tarefas e, para fazer isso, apresenta o token de acesso para o WebAPI obtido de AD FS. A API da Web corresponde ao valor de público-alvo para verificar se o token é destinado a ele e verifica a assinatura de token usando as informações dos metadados de Federação.

![Entrar](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  
