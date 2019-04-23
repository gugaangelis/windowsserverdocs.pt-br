---
title: Criar um aplicativo cliente nativo usando clientes públicos OAuth com o AD FS 2016
description: Um passo a passo que fornece instruções para criar um aplicativo cliente nativo usando clientes públicos OAuth e o AD FS 2016
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 6302e282ebf7f84f741835d52333b99bec945f91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846477"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016"></a>Criar um aplicativo cliente nativo usando clientes públicos OAuth com o AD FS 2016

## <a name="overview"></a>Visão geral

Este artigo mostra como criar um aplicativo nativo que interage com uma API Web protegida pelo AD FS 2016.

1. O .net aplicativo WPF TodoListClient usa o Active Directory autenticação ADAL (biblioteca) para obter um token de acesso JWT do Azure Active Directory (Azure AD) por meio do protocolo OAuth 2.0
2. O token de acesso é usado como um token de portador para autenticar o usuário ao chamar o ponto de extremidade da API web TodoListService /todolist.
 Podemos será usando o exemplo de aplicativo do Azure AD aqui e, em seguida, modificá-lo para o AD FS 2016.

![Visão geral do aplicativo](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>Pré-requisitos
A seguir é uma lista de pré-requisitos são necessários antes de concluir este documento. Este documento pressupõe que o AD FS foi instalado e um farm do AD FS tiver sido criado. 

* Ferramentas de cliente do GitHub 
* O AD FS no Windows Server 2016
* Visual Studio 2013 ou posterior.

## <a name="creating-the-sample-walkthrough"></a>Criando o passo a passo de exemplo

### <a name="create-the-application-group-in-ad-fs"></a>Crie o grupo de aplicativos no AD FS

1. No gerenciamento do AD FS, clique duas vezes em **grupos de aplicativos** e selecione **adicionar grupo de aplicativos**.

2. No Assistente de grupo do aplicativo, para o nome, digite qualquer nome que você preferir, por exemplo, NativeToDoListAppGroup. Selecione o **aplicativo nativo acessando uma API web** modelo. Clique em **Avançar**.
 ![Adicionar grupo de aplicativos](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. Sobre o **aplicativo nativo** página, observe o identificador gerado pelo AD FS. Esta é a id com a qual o AD FS reconheça o aplicativo cliente público. Cópia de **identificador de cliente** valor. Ele será usado posteriormente como o valor para **ida: ClientId** no código do aplicativo. Se desejar, você pode fornecer qualquer identificador personalizado aqui. O URI de redirecionamento for qualquer valor arbitrário, como exemplo, coloque https://ToDoListClient ![aplicativo nativo](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. Sobre o **configurar a API da Web** , defina o valor do identificador para a API da Web. Neste exemplo, isso deve ser o valor da **URL do SSL** onde o aplicativo Web deve estar sendo executado. Você pode obter esse valor clicando-se nas propriedades do projeto TooListServer na solução. Isso será usado posteriormente como o **todo:TodoListResourceId** o valor **App. config** arquivo do aplicativo cliente nativo e também como o **todo:TodoListBaseAddress**.
![API da Web](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. Percorra os **Aplicar política de controle de acesso** e **configurar permissões do aplicativo** com os valores padrão em vigor. A página de resumo deve ter aparência abaixo.
![Resumo](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)
 
Clique em Avançar e, em seguida, conclua o assistente.

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>Adicione a declaração NameIdentifier à lista de declarações emitidas
O aplicativo de demonstração usa o valor na declaração do NameIdentifier em vários locais. Ao contrário do AD do Azure, o AD FS não emitirá uma declaração NameIdentifier por padrão. Portanto, precisamos adicionar uma regra de declaração para emitir a declaração NameIdentifier para que o aplicativo pode usar o valor correto. Neste exemplo, o nome fornecido do usuário é emitido como o valor de NameIdentifier do usuário no token.
Para configurar a regra de declaração, abra o grupo de aplicativo que acabou de criar e clique duas vezes na API Web. Selecione a guia regras de transformação de emissão e, em seguida, clique no botão Adicionar regra. O tipo de regra de declaração, escolha a regra de declaração personalizada e, em seguida, adicione a regra de declaração, conforme mostrado abaixo.
![Regra de declaração do NameIdentifier](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>Modificar o código do aplicativo

#### <a name="modify-todolistclient"></a>Modificar o ToDoListClient

Este projeto na solução representa o aplicativo cliente nativo. É preciso certificar-se de que o aplicativo cliente sabe:

1. Onde ir para obter o usuário autenticado quando necessário?
2. O que é a ID que o cliente precisa fornecer para a autoridade de autenticação (AD FS)?
3. O que é a ID do recurso que estamos pedindo para o token de acesso?
4. O que é o endereço básico da API Web?

As alterações de código a seguir são necessárias para obter as informações acima para o aplicativo cliente nativo.

**App.config**

* Adicionar a chave **ida: autoridade** com o valor que descrevam o AD serviço FS, o exemplo, https://fs.contoso.com/adfs/
* Modifique **ida: ClientId** chave com o valor da **aplicativo nativo** página durante a criação do grupo de aplicativos no AD FS. Para ex 3f07368b-6efd-4f50-a330-d93853f4c855
* Modificar a **todo:TodoListBaseAddress** para o endereço básico da API da Web, por exemplo, https://localhost:44321/
* Defina o valor de **ida: RedirectUri** para o valor que você coloca na página do aplicativo nativo durante adicionar grupo de aplicativos no AD FS.
* Para facilitar a leitura, você pode remover / comentar a chave para **ida: locatário** e **ida: AADInstance**.

**MainWindow.xaml.cs**

* Remova a linha para aadInstance
* Adicione o valor para a autoridade como mostrado abaixo

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* Exclua a linha para a criação de **autoridade** valor da aadInstance e locatário

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* Na função **MainWindow**, altere a instanciação authContext como

        authContext = new AuthenticationContext(authority,false);

    ADAL não oferece suporte a validação do AD FS como autoridade e, portanto, precisamos passar um sinalizador de valor falso para o parâmetro validateAuthority.    

#### <a name="modify-todolistservice"></a>Modificar TodoListService
Dois arquivos precisam alterações neste projeto – Web. config e Startup.Auth.cs. Alterações de Web. config são necessárias para obter os valores corretos dos parâmetros. Alterações de Startup.Auth.cs são necessárias para definir a API da Web para autenticar no AD FS, em vez do Azure AD.

**Web.config**

* Excluir a chave **ida: locatário** conforme ela não é necessária
* Adicione a chave da **ida: autoridade** com um valor que indica o FQDN da federação de serviço, o exemplo, https://fs.contoso.com/adfs/
* Modificar chave **ida: público-alvo** com o valor do identificador de API da Web que você especificou na **configurar a API da Web** página durante a adicionar o grupo de aplicativos no AD FS.
* Adicionar chave **ida: AdfsMetadataEndpoint** valor correspondente para a URL de metadados de Federação do AD FS com o serviço para ex: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

**Startup.Auth.cs**

Modifique a função ConfigureAuth conforme mostrado a seguir

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

Essencialmente, estamos configurando a autenticação para usar o AD FS e fornecem mais informações sobre os metadados do AD FS e para validar o token, a declaração de público-alvo deve ser o valor esperado pela API da Web.
Executando o aplicativo

1. Na solução NativeClient-DotNet, clique com botão direito e vá para propriedades. Altere o projeto de inicialização, como mostrado abaixo para projetos de inicialização de vários e defina tanto TodoListClient como TodoListService inicial.
![Propriedades da solução](media/native-client-with-ad-fs-2016/solutionproperties.png)
 
2.  Pressione o botão F5 ou selecione Depurar > continuar na barra de menus. Isso iniciará o aplicativo nativo e a API da Web. Clique no botão entrar no aplicativo nativo e ele será um logon interativo de pop-up de AL AD redirecionar para o serviço do AD FS. Insira as credenciais de um usuário válido.
![Entrar](media/native-client-with-ad-fs-2016/sign-in.png)
 
Nesta etapa, o aplicativo nativo redirecionado para o AD FS e tem um token de ID e um token de acesso para a API da Web

3.  Insira um item na caixa de texto e clique em Adicionar item. Nesta etapa, o aplicativo acessa a API da Web para adicionar o item e para fazer isso, apresenta o token de acesso para a API Web obtida do AD FS. A API da Web corresponde ao valor de público-alvo para garantir que o token é destinado para ele e verifica a assinatura do token usando as informações de metadados de Federação.
 
![Entrar](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>Próximas etapas
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  