---
title: AD FS a API Web de chamada de aplicativo nativo do MSAL
description: Ganhe instruções para criar um aplicativo nativo autenticado por usuários AD FS 2019 e adquirir tokens usando a biblioteca MSAL para chamar APIs da Web.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ff76a6dffd66296a02cffcbd79bc6dfadc91c14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407798"
---
# <a name="scenario-native-app-calling-web-api"></a>Cenário: Aplicativo nativo chamando API Web 
>Aplica-se a: AD FS 2019 e posterior 
 
Saiba como criar um aplicativo nativo autenticando usuários com autenticação AD FS 2019 e adquirindo tokens usando a [biblioteca MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) para chamar APIs da Web.  
 
Antes de ler este artigo, você deve estar familiarizado com os [conceitos de AD FS](../ad-fs-openid-connect-oauth-concepts.md) e o fluxo de concessão de código de [autorização](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)
 
## <a name="overview"></a>Visão geral 
 
 ![Visão geral](media/adfs-msal-native-app-web-api/native1.png)

Nesse fluxo, você adiciona autenticação ao seu aplicativo nativo (cliente público), que, portanto, pode conectar usuários e chamar uma API da Web. Para chamar uma API Web de um aplicativo nativo que assina usuários, você pode usar o método de aquisição de token [AcquireTokenInteractive](https://docs.microsoft.com/en-us/dotnet/api/microsoft.identity.client.ipublicclientapplication.acquiretokeninteractive?view=azure-dotnet#Microsoft_Identity_Client_IPublicClientApplication_AcquireTokenInteractive_System_Collections_Generic_IEnumerable_System_String__) do MSAL. Para habilitar essa interação, o MSAL aproveita um navegador da Web. 

 
Para entender melhor como configurar um aplicativo nativo no ADFS para adquirir o token de acesso interativamente, vamos usar um exemplo disponível [aqui](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi) e explicando as etapas de configuração de código e de registro do aplicativo.  
 

## <a name="pre-requisites"></a>Pré-requisitos 


- Ferramentas de cliente do GitHub 
- AD FS 2019 ou posterior configurados e em execução 
- Visual Studio 2013 ou posterior 
 

## <a name="app-registration-in-ad-fs"></a>Registro de aplicativo no AD FS 
Esta seção mostra como registrar o aplicativo nativo como um cliente público e uma API Web como uma terceira parte confiável (RP) no AD FS 

  1. Em **Gerenciamento de AD FS**, clique com o botão direito do mouse em **grupos de aplicativos** e selecione **Adicionar grupo de aplicativos**.   
  
  2. No assistente de grupo de aplicativos, para o **nome** , insira **NativeAppToWebApi** e, em **aplicativos cliente-servidor** , selecione o **aplicativo nativo acessando um modelo de API Web** . Clique em **Avançar**.  
  
      ![Reg do aplicativo](media/adfs-msal-native-app-web-api/native2.png)  

  3. Copie o valor do **identificador de cliente** . Ele será usado posteriormente como o valor de **ClientID** no arquivo **app. config** do aplicativo. Insira o seguinte para o **URI de redirecionamento:** https://ToDoListClient. Clique em **Adicionar** . Clique em **Avançar**.  
 
     ![Reg do aplicativo](media/adfs-msal-native-app-web-api/native3.png) 

  4. Na tela configurar API da Web, insira o **identificador:** https://localhost:44321/. Clique em **Adicionar** . Clique em **Avançar**. Esse valor será usado posteriormente nos arquivos **app. config** e **Web. config** do aplicativo.
 
     ![Reg do aplicativo](media/adfs-msal-native-app-web-api/native4.png)   
  
  5. Na tela aplicar política de controle de acesso, selecione **permitir todos** e clique em **Avançar**. 
  
     ![Reg do aplicativo](media/adfs-msal-native-app-web-api/native5.png)   
  
  6. Na tela configurar permissões de aplicativo, verifique se **OpenID** está selecionado e clique em **Avançar**.  
     
     ![Reg do aplicativo](media/adfs-msal-native-app-web-api/native6.png) 

  7. Na tela Resumo, clique em **Avançar**.
  
  8. Na tela concluir, clique em **fechar**. 
  
  9. Em gerenciamento de AD FS, clique em **grupos de aplicativos** e selecione grupo de aplicativos **NativeAppToWebApi** . Clique com o botão direito do mouse e selecione **Propriedades**.
  
      ![Reg do aplicativo](media/adfs-msal-native-app-web-api/native7.png)

  10. Na tela de propriedades do NativeAppToWebApi, selecione **NativeAppToWebApi – API Web** em **API da Web** e clique em **Editar..** . 
  
      ![Reg do aplicativo](media/adfs-msal-native-app-web-api/native8.png) 

  11. Na tela NativeAppToWebApi – Propriedades da API Web, selecione a guia **regras de transformação de emissão** e clique em **Adicionar regra..** . 
  
      ![Reg do aplicativo](media/adfs-msal-native-app-web-api/native9.png) 

  12. Em Adicionar Assistente de regra de declaração de transformação, selecione **transformar uma declaração de entrada** do **modelo de regra de declaração:** menu suspenso e clique em **Avançar**.  
  
      ![Reg do aplicativo](media/adfs-msal-native-app-web-api/native10.png) 

  13. Insira **NameID** no campo **nome da regra de declaração:** . Selecione o **nome** para o **tipo de declaração de entrada:** , **ID de nome** para tipo de **declaração de saída:** e **nome comum** para **formato de ID de nome de saída:** . Clique em **concluir**.
  
      ![Reg do aplicativo](media/adfs-msal-native-app-web-api/native11.png) 

  14. Clique em OK em NativeAppToWebApi – tela de propriedades da API Web e, em seguida, NativeAppToWebApi tela de propriedades.  
 
## <a name="code-configuration"></a>Configuração de código 
Esta seção mostra como configurar um aplicativo nativo para o usuário de entrada e recuperar o token para chamar a API da Web 

1. Baixe o [exemplo daqui](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi) 

2. Abrir o exemplo usando o Visual Studio 

3. Abra o arquivo app. config. Modifique o seguinte: 
   - Ida: Authority-insira https://[seu nome de host do AD FS]/ADFS
   - Ida: ClientId – Insira o valor do **identificador de cliente** de #3 no registro do aplicativo na seção AD FS acima. 
   - Ida: RedirectUri-Insira o valor do **URI de redirecionamento** de #3 no registro do aplicativo na seção AD FS acima.
   - todo: TodoListResourceId – Insira o valor do **identificador** de #4 no registro do aplicativo na seção AD FS acima 
   - Ida: todo: TodoListBaseAddress-Insira o valor do **identificador** de #4 no registro do aplicativo na seção AD FS acima. 
 
     ![configuração de código](media/adfs-msal-native-app-web-api/native12.png)

 4. Abra o arquivo Web. config. Modifique o seguinte: 
    - Ida: Audience-Insira o valor do **identificador** de #4 no registro do aplicativo na seção AD FS acima 
    - Ida AdfsMetadataEndpoint-insira https://[seu nome de host do AD FS]/FederationMetadata/2007-06/FederationMetadata.xml 
    
      ![configuração de código](media/adfs-msal-native-app-web-api/native13.png)
 
  
## <a name="test-the-sample"></a>Testar o exemplo 
Esta seção mostra como testar o exemplo configurado acima. 

  1. Depois que as alterações de código forem feitas para recompilar a solução 
 
  2. No Visual Studio, clique com o botão direito do mouse em solução e selecione **definir projetos de inicialização...**  
 
     ![Teste de aplicativo](media/adfs-msal-native-app-web-api/native14.png)

  3. Nas páginas de propriedades, verifique se a **ação** está definida como **Iniciar** para cada um dos projetos 
      
     ![Teste de aplicativo](media/adfs-msal-native-app-web-api/native15.png)

  4. Na parte superior do Visual Studio, clique na seta verde.  
 
     ![Teste de aplicativo](media/adfs-msal-native-app-web-api/native16.png)

  5. Na tela principal do aplicativo nativo, clique em **entrar**.  
  
     ![Teste de aplicativo](media/adfs-msal-native-app-web-api/native17.png)

    Se você não vir a tela do aplicativo nativo, pesquise e remova os arquivos * msalcache. bin da pasta em que o repositório do projeto é salvo em seu sistema. 

  6. Você será direcionado novamente para a página de entrada AD FS. Vá em frente e entre. 
  
      ![Teste de aplicativo](media/adfs-msal-native-app-web-api/native18.png)

  7. Depois de conectado, insira texto **Compilar aplicativo nativo para API Web** no **criar um item de tarefas pendentes**. Clique em **Adicionar item**.  Isso chamará o **serviço de lista de tarefas pendentes (API Web)** e adicionará o item no cache. 
    
       ![Teste de aplicativo](media/adfs-msal-native-app-web-api/native19.png)
 
## <a name="next-steps"></a>Próximas etapas
[Fluxos e cenários de aplicativo do AD FS OpenID Connect/OAuth](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 