---
title: AD FS aplicativo Web MSAL (aplicativo de servidor) chamando APIs da Web
description: Saiba como criar um aplicativo Web que assina usuários autenticados pelo AD FS 2019.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 700ba841dba8e022f47d906b719f57befafc093c
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966678"
---
# <a name="scenario-web-app-server-app-calling-web-api"></a>Cenário: aplicativo Web (aplicativo de servidor) chamando API Web 
>Aplica-se a: AD FS 2019 e posterior 
 
Saiba como criar um aplicativo Web que assina usuários autenticados pelo AD FS 2019 e adquirindo tokens usando a [biblioteca MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) para chamar APIs da Web.  
 
Antes de ler este artigo, você deve estar familiarizado com os [conceitos de AD FS](../ad-fs-openid-connect-oauth-concepts.md) e o fluxo de concessão de código de [autorização](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)
 
## <a name="overview"></a>Visão geral 
 
![Visão geral da API Web de chamada de aplicativo Web](media/adfs-msal-web-app-web-api/webapp1.png)

Nesse fluxo, você adiciona autenticação ao seu aplicativo Web (aplicativo de servidor), que, portanto, pode conectar usuários e chamar uma API da Web. No aplicativo Web, para chamar a API Web, use o método de aquisição de token [AcquireTokenByAuthorizationCode](/dotnet/api/microsoft.identity.client.acquiretokenbyauthorizationcodeparameterbuilder?view=azure-dotnet) da MSAL. Você usará o fluxo de código de autorização, armazenando o token adquirido no cache de token. Em seguida, o controlador adquirirá tokens silenciosamente do cache sempre que necessário. A MSAL atualiza o token, se necessário. 

Aplicativos Web que chamam APIs da Web: 


- são aplicativos cliente confidenciais. 
- é por isso que eles registraram um segredo (segredo compartilhado do aplicativo, certificado ou conta do AD) com AD FS. Esse segredo é passado durante a chamada para AD FS para obter um token.  

Para entender melhor como registrar um aplicativo Web no ADFS e configurá-lo para adquirir tokens para chamar uma API Web, vamos usar um exemplo disponível [aqui](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi) e, em seguida, as etapas de registro do aplicativo e de configuração de código.  

 
## <a name="pre-requisites"></a>Pré-requisitos 

- Ferramentas de cliente do GitHub 
- AD FS 2019 ou posterior configurados e em execução 
- Visual Studio 2013 ou posterior. 
 
## <a name="app-registration-in-ad-fs"></a>Registro de aplicativo no AD FS 
Esta seção mostra como registrar o aplicativo Web como um cliente confidencial e uma API Web como uma terceira parte confiável (RP) no AD FS. 

  1. Em gerenciamento de AD FS, clique com o botão direito do mouse em **grupos de aplicativos** e selecione **Adicionar grupo de aplicativos**.  
  2. No assistente de grupo de aplicativos, para o **nome** , insira **WebAppToWebApi** e, em **aplicativos cliente-servidor** , selecione o **aplicativo de servidor acessando um modelo de API Web** . Clique em **Próximo**.  
  
      ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp2.png)
  
  3. Copie o valor do **identificador de cliente** . Ele será usado posteriormente como o valor de **ida: ClientID** no arquivo de **Web.config** de aplicativos. Insira o seguinte para o **URI de redirecionamento:**  -  https://localhost:44326 . Clique em Adicionar. Clique em **Próximo**. 
  
      ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp3.png)
  
  4. Na tela configurar credenciais do aplicativo, faça um check-in **gerar um segredo compartilhado** e copie o segredo. Isso será usado posteriormente como o valor de **ida: ClientSecret** no arquivo de **Web.config** de aplicativos. Clique em **Próximo**.  
  
      ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp4.png)
  
  5. Na tela configurar API da Web, insira o **identificador:** https://webapi . Clique em **Adicionar**. Clique em **Próximo**. Esse valor será usado posteriormente para **ida: GraphResourceId** no arquivo de **Web.config** de aplicativos. 
  
      ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp5.png)
  
  6. Na tela aplicar política de controle de acesso, selecione **permitir** todos e clique em **Avançar**. 
  
      ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp6.png)
  
  7. Na tela configurar permissões de aplicativo, verifique se **OpenID** e **user_impersonation** estão selecionados e clique em **Avançar**. 
  
      ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp7.png)
  
  8. Na tela Resumo, clique em **Avançar**. 
  
  9. Na tela concluir, clique em **fechar**.



## <a name="code-configuration"></a>Configuração de código 

Esta seção mostra como configurar um aplicativo Web ASP.NET para o usuário de entrada e recuperar o token para chamar a API da Web 

  1. Baixe o [exemplo daqui](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi)   
  
  2. Abrir o exemplo usando o Visual Studio 
  
  3. Abra o arquivo web.config. Modifique o seguinte: 
       - Ida: ClientId – Insira o valor do **identificador de cliente** de #3 no registro do aplicativo na seção AD FS acima. 
       - Ida: ClientSecret-Insira o valor **secreto** de #4 no registro do aplicativo na seção AD FS acima. 
       - Ida: RedirectUri-Insira o valor do **URI de redirecionamento** de #3 no registro do aplicativo na seção AD FS acima. 
       - Ida: Authority-insira https://[seu nome de host do AD FS]/ADFS. Por exemplo, https://adfs.contoso.com/adfs 
       - Ida: Resource – Insira o valor do **identificador** de #5 no registro do aplicativo na seção AD FS acima. 
      
          ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp8.png)
 
 
### <a name="test-the-sample"></a>O exemplo de teste 
Esta seção mostra como testar o exemplo configurado acima. 

  1. Depois que as alterações de código forem feitas para recompilar a solução 
  
  2. Na parte superior do Visual Studio, verifique se o Internet Explorer está selecionado e clique na seta verde. 
  
      ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp9.png)

  3. Na Home Page, clique em entrar. 
  
      ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp10.png)

  4. Você será direcionado novamente para a página de entrada AD FS. Vá em frente e entre. 
  
      ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp11.png)

  5. Depois de conectado, clique em token de acesso.  
  
      ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp12.png)

  6. Clicar no token de acesso obterá as informações de token de acesso chamando a API da Web 
  
      ![Adicionar grupo de aplicativos](media/adfs-msal-web-app-web-api/webapp13.png)
 
 ## <a name="next-steps"></a>Próximas etapas
[Fluxos e cenários de aplicativo do AD FS OpenID Connect/OAuth](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 
