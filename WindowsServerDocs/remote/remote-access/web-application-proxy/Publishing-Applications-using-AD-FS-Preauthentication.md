---
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: Publicar aplicativos usando a Pré-autenticação do AD FS
description: ''
author: kgremban
manager: femila
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: c7dab1dbf97d2dcbda1fe0375e61300f2a1cc373
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862237"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>Publicar aplicativos usando a Pré-autenticação do AD FS

>Aplica-se a: Windows Server 2016

**Este conteúdo é relevante para a versão local do Proxy de aplicativo Web. Para habilitar o acesso seguro a aplicativos locais pela nuvem, consulte o [conteúdo de Proxy de aplicativo do Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
Este tópico descreve como publicar aplicativos por meio do Proxy de aplicativo Web usando o Active Directory Federation Services (AD FS) pré-autenticação.  
  
Para todos os tipos de aplicativo que você pode publicar usando a pré-autenticação do AD FS, você deve adicionar um objeto de confiança para o serviço de federação de terceira parte confiável do AD FS.  
  
O fluxo geral de pré-autenticação do AD FS é da seguinte maneira:  
  
> [!NOTE]  
> Esse fluxo de autenticação não é aplicável para clientes que usam aplicativos da Microsoft Store.  
  
1.  O dispositivo cliente tenta acessar um aplicativo web publicado em uma URL de recurso específico; Por exemplo https://app1.contoso.com/.  
  
    A URL de recurso é um endereço público em que o Proxy de aplicativo Web escuta HTTPS para solicitações de entrada.  
  
    Se HTTP para redirecionamento a HTTPS estiver habilitado, o Proxy de aplicativo Web também escutará solicitações HTTP de entrada.  
  
2.  Proxy de aplicativo Web redireciona a solicitação HTTPS ao servidor do AD FS com os parâmetros codificados de URL, incluindo a URL de recurso e o appRealm (um identificador de terceiros terceira parte confiável).  
  
    O usuário autentica usando o método de autenticação exigido pelo servidor do AD FS; Por exemplo, nome de usuário e senha, autenticação de dois fatores com uma senha de uso único e assim por diante.  
  
3.  Depois que o usuário é autenticado, o servidor do AD FS emite uma segurança token, o "token de borda', que contém as informações a seguir e redireciona os HTTPS solicitação volta para o servidor de Proxy de aplicativo Web:  
  
    -   O identificador de recurso que o usuário tentou acessar.  
  
    -   A identidade do usuário como um nome de usuário principal (UPN).  
  
    -   A expiração da aprovação de concessão de acesso; isto é, é concedido acesso ao usuário por um período de tempo limitado, após o qual é exigido que eles se autentiquem novamente.  
  
    -   Assinatura das informações no token de borda.  
  
4.  Proxy de aplicativo Web recebe a solicitação HTTPS redirecionada do servidor do AD FS com o token de borda e valida e usa o token da seguinte maneira:  
  
    -   Valida que a assinatura do token de borda é do serviço de Federação está configurado na configuração de Proxy de aplicativo Web.  
  
    -   Valida que o token foi emitido para o aplicativo correto.  
  
    -   Valida que o token não expirou.  
  
    -   Usa a identidade do usuário quando necessário; por exemplo, para obter um tíquete de Kerberos se o servidor de back-end estiver configurado para usar a autenticação integrada do Windows.  
  
5.  Se o token de borda for válido, o Proxy de aplicativo Web encaminha a solicitação HTTPS ao aplicativo web publicado usando HTTP ou HTTPS.  
  
6.  O cliente agora tem acesso ao aplicativo Web publicado; no entanto, o aplicativo publicado pode estar configurado para exigir que o usuário execute autenticação adicional. Se, por exemplo, o aplicativo Web publicado for um site do SharePoint e não exigir autenticação adicional, o usuário verá o site do SharePoint no navegador.  
  
7.  Proxy de aplicativo Web salva um cookie no dispositivo cliente. O cookie é usado pelo Proxy de aplicativo Web para identificar esta sessão já foi pré-autenticada e que nenhuma pré-autenticação adicional é necessária.  
  
> [!IMPORTANT]  
> Ao configurar a URL externa e a URL do servidor de back-end, você deve incluir o nome de domínio totalmente qualificado (FQDN), e não um endereço IP.  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_1.1"></a>Publicar um aplicativo baseado em declarações para clientes de navegador da Web  
Para publicar um aplicativo que usa declarações para autenticação, você deve adicionar ao Serviço de Federação um objeto de confiança de terceira parte confiável para o aplicativo.  
  
Quando publicar aplicativos baseados em declarações e acessar o aplicativo em um navegador, o fluxo de autenticação geral é da seguinte maneira:  
  
1.  O cliente tenta acessar um aplicativo baseado em declarações usando um navegador da web; Por exemplo, https://appserver.contoso.com/claimapp/.  
  
2.  O navegador da web envia uma solicitação HTTPS para o servidor de Proxy de aplicativo Web que redireciona a solicitação para o servidor do AD FS.  
  
3.  Servidor do AD FS autentica o usuário e o dispositivo e redireciona a solicitação de volta para o Proxy de aplicativo Web. A solicitação agora contém o token de borda. Servidor do AD FS adiciona um cookie de (SSO) logon único para a solicitação porque o usuário já executou a autenticação no servidor do AD FS.  
  
4.  Proxy de aplicativo Web valida o token, adiciona seu próprio cookie e encaminha a solicitação para o servidor de back-end.  
  
5.  O servidor de back-end redireciona a solicitação para o servidor do AD FS para obter um token de segurança do aplicativo.  
  
6.  A solicitação é redirecionada para o servidor de back-end pelo servidor do AD FS. A solicitação agora contém o token de aplicativo e o cookie de SSO. O usuário recebe permissão para acessar o aplicativo e não é necessário que digite um nome de usuário ou uma senha.  
  
Este procedimento descreve como publicar um aplicativo baseado em declarações, como um site do SharePoint, que será acessado por clientes do navegador da Web. Antes de começar, certifique-se de que você tenha feito o seguinte:  
  
-   Criou uma terceira parte confiável para o aplicativo no console de gerenciamento do AD FS.  
  
-   Verificado se um certificado no servidor de Proxy de aplicativo Web é adequado para o aplicativo que você deseja publicar.  
  

  
#### <a name="to-publish-a-claims-based-application"></a>Para publicar um aplicativo baseado em declarações  
  
1.  No servidor de Proxy de aplicativo Web, no console de gerenciamento de acesso remoto, nos **navegação** painel, clique em **Proxy de aplicativo Web**e, em seguida, no **tarefas** painel, clique em **Publicar**.  
  
2.  No **Assistente para Publicar Novo Aplicativo**, na página de **Boas-vindas**, clique em **Avançar**.  
  
3.  No **pré-autenticação** , clique em **serviços de Federação do Active Directory (AD FS)** e, em seguida, clique em **próxima**.  
  
4.  Na página **Clientes com Suporte** selecione **Web e MSOFBA**, e, em seguida, clique em **Avançar**.  
  
5.  Na página **Terceira Parte Confiável**, na lista de terceiras partes confiáveis, selecione a terceira parte confiável para o aplicativo que pretende publicar e clique em **Avançar**.  
  
6.  Na página **Configurações de publicação** , faça o seguinte e clique em **Avançar**:  
  
    -   Na caixa **Nome**, digite um nome amigável para o aplicativo.  
  
        Esse nome é usado apenas na lista de aplicativos publicados no console de Gerenciamento de Acesso Remoto.  
  
    -   Na caixa **URL Externa**, insira a URL externa para esse aplicativo; por exemplo, https://sp.contoso.com/app1/.  
  
    -   Na lista **Certificado externo**, selecione um certificado cujo assunto abranja a URL externa.  
  
    -   Na caixa **URL do servidor de back-end**, digite a URL do servidor de back-end. Observe que esse valor é automaticamente inserido quando você insere a URL externa e você deve alterá-lo somente se a URL do servidor de back-end for diferente; Por exemplo, https://sp/app1/.  
  
        > [!NOTE]  
        > Proxy de aplicativo Web pode converter nomes de host em URLs, mas não pode converter nomes de caminho. Portanto, você pode inserir nomes de host diferentes, mas deve inserir o mesmo nome de caminho. Por exemplo, você pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL do servidor de back-end de https://app-server/app1/. No entanto, você não pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL do servidor de back-end de https://apps.contoso.com/internal-app1/.  
  
7.  Na página **Confirmação** , examine as configurações e clique em **Publicar**. É possível copiar o comando do PowerShell para configurar aplicativos publicados adicionais.  
  
8.  Na página **Resultados**, verifique se o aplicativo foi publicado com êxito e clique em **Fechar**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://sp.contoso.com/app1/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://sp.contoso.com/app1/'  
    -Name 'SP'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'SP_Relying_Party'  
```  
  
## <a name="BKMK_1.2"></a>Publicar um aplicativo do Windows integrada com base em autenticado para clientes de navegador da Web  
Proxy de aplicativo Web pode ser usado para publicar aplicativos que usam autenticação integrada Windows; ou seja, o Proxy de aplicativo Web executa a pré-autenticação conforme necessário e, em seguida, pode executar o SSO para o aplicativo publicado que usa a autenticação do Windows integrado. Para publicar um aplicativo que usa autenticação integrada do Windows, você deve adicionar ao Serviço de Federação um objeto de confiança de terceira parte confiável sem reconhecimento de declarações para o aplicativo.  
  
Para permitir que o Proxy de aplicativo Web para executar logon único (SSO) e execute a delegação de credenciais usando o Kerberos, a delegação restrita, o Proxy de aplicativo Web server deve ser associado a um domínio. Ver [planejar o Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Para permitir que os usuários acessem aplicativos que usam a autenticação do Windows integrada, o servidor de Proxy de aplicativo Web deve ser capaz de fornecer a delegação para os usuários para o aplicativo publicado. Você pode fazer isso no controlador de domínio de qualquer aplicativo. Você também pode fazer isso no servidor de back-end se ele estiver em execução no Windows Server 2012 R2 ou Windows Server 2012. Para obter mais informações, consulte [Novidades na autenticação Kerberos](https://technet.microsoft.com/library/hh831747.aspx).  
  
Para obter uma explicação de como configurar o Proxy de aplicativo Web para publicar um aplicativo usando a autenticação do Windows integrada, consulte [configurar um site para usar a autenticação do Windows integrada](https://technet.microsoft.com/library/dn280943.aspx#BKMK_3).  
  
Ao usar a autenticação do Windows integrada aos servidores de back-end, a autenticação entre o Proxy de aplicativo Web e o aplicativo publicado não é baseada em declarações, em vez disso, ele usa a delegação restrita de Kerberos para autenticar usuários finais para o aplicativo. O fluxo geral é descrito abaixo:  
  
1.  O cliente tenta acessar um aplicativo não baseado em declarações usando um navegador da web; Por exemplo, https://appserver.contoso.com/nonclaimapp/.  
  
2.  O navegador da web envia uma solicitação HTTPS para o servidor de Proxy de aplicativo Web que redireciona a solicitação para o servidor do AD FS.  
  
3.  Servidor do AD FS autentica o usuário e redireciona a solicitação de volta para o Proxy de aplicativo Web. A solicitação agora contém o token de borda.  
  
4.  Proxy de aplicativo Web valida o token.  
  
5.  Se o token for válido, o Proxy de aplicativo Web obtém um tíquete Kerberos do controlador de domínio em nome do usuário.  
  
6.  Proxy de aplicativo Web adiciona o tíquete de Kerberos à solicitação como parte do token simples e o mecanismo de negociação GSS-API protegidas (SPNEGO) e encaminha a solicitação para o servidor de back-end. A solicitação contém o tíquete de Kerberos; portanto, o usuário ter permissão para acessar o aplicativo sem necessidade de autenticação adicional.  
  
Este procedimento descreve como publicar um aplicativo que usa a autenticação integrada do Windows, como o Outlook Web App, que será acessado por clientes do navegador da Web. Antes de começar, certifique-se de que você tenha feito o seguinte:  
  
-   Criado um sem reconhecimento de declarações terceira parte confiável para o aplicativo no console de gerenciamento do AD FS.  
  
-   Configurado o servidor de back-end para dar suporte à delegação restrita de Kerberos no controlador de domínio ou usando o cmdlet Set-ADUser com o parâmetro -PrincipalsAllowedToDelegateToAccount. Observe que se o servidor de back-end estiver em execução no Windows Server 2012 R2 ou Windows Server 2012, você também pode executar esse comando do PowerShell no servidor de back-end.  
  
-   Verificado se de que os servidores de Proxy de aplicativo Web são configurados para delegação aos nomes da entidade de serviço dos servidores de back-end.  
  
-   Verificado se um certificado no servidor de Proxy de aplicativo Web é adequado para o aplicativo que você deseja publicar.  
  
 
  
#### <a name="to-publish-a-non-claims-based-application"></a>Para publicar um aplicativo que não é baseado em declarações  
  
1.  No servidor de Proxy de aplicativo Web, no console de gerenciamento de acesso remoto, nos **navegação** painel, clique em **Proxy de aplicativo Web**e, em seguida, no **tarefas** painel, clique em **Publicar**.  
  
2.  No **Assistente para Publicar Novo Aplicativo**, na página de **Boas-vindas**, clique em **Avançar**.  
  
3.  No **pré-autenticação** , clique em **serviços de Federação do Active Directory (AD FS)** e, em seguida, clique em **próxima**.  
  
4.  Na página **Clientes com Suporte** selecione **Web e MSOFBA**, e, em seguida, clique em **Avançar**.  
  
5.  Na página **Terceira Parte Confiável**, na lista de terceiras partes confiáveis, selecione a terceira parte confiável para o aplicativo que pretende publicar e clique em **Avançar**.  
  
6.  Na página **Configurações de publicação** , faça o seguinte e clique em **Avançar**:  
  
    -   Na caixa **Nome**, digite um nome amigável para o aplicativo.  
  
        Esse nome é usado apenas na lista de aplicativos publicados no console de Gerenciamento de Acesso Remoto.  
  
    -   Na caixa **URL Externa**, insira a URL externa para esse aplicativo; por exemplo, https://owa.contoso.com/.  
  
    -   Na lista **Certificado externo**, selecione um certificado cujo assunto abranja a URL externa.  
  
    -   Na caixa **URL do servidor de back-end**, digite a URL do servidor de back-end. Observe que esse valor é automaticamente inserido quando você insere a URL externa e você deve alterá-lo somente se a URL do servidor de back-end for diferente; Por exemplo, https://owa/.  
  
        > [!NOTE]  
        > Proxy de aplicativo Web pode converter nomes de host em URLs, mas não pode converter nomes de caminho. Portanto, você pode inserir nomes de host diferentes, mas deve inserir o mesmo nome de caminho. Por exemplo, você pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL do servidor de back-end de https://app-server/app1/. No entanto, você não pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL do servidor de back-end de https://apps.contoso.com/internal-app1/.  
  
    -   Na caixa **SPN do servidor de back-end**, insira o nome da entidade de serviço para o servidor de back-end; por exemplo, HTTP/owa.contoso.com.  
  
7.  Na página **Confirmação** , examine as configurações e clique em **Publicar**. É possível copiar o comando do PowerShell para configurar aplicativos publicados adicionais.  
  
8.  Na página **Resultados**, verifique se o aplicativo foi publicado com êxito e clique em **Fechar**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerAuthenticationSpn 'HTTP/owa.contoso.com'  
    -BackendServerURL 'https://owa.contoso.com/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://owa.contoso.com/'  
    -Name 'OWA'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'Non-Claims_Relying_Party'  
```  
  
## <a name="BKMK_1.3"></a>Publicar um aplicativo que usa o MS-OFBA  
Proxy de aplicativo Web dá suporte ao acesso de clientes do Microsoft Office, como o Microsoft Word que acessa documentos e dados em servidores de back-end. A única diferença entre esses aplicativos e um navegador padrão é que o redirecionamento para o STS é feito não pelo redirecionamento HTTP regular, mas com cabeçalhos especiais de MS-OFBA conforme especificado em: [ https://msdn.microsoft.com/library/dd773463(v=office.12).aspx ](https://msdn.microsoft.com/library/dd773463(v=office.12).aspx). O aplicativo de back-end pode ser declarações ou IWA.   
Para publicar um aplicativo para clientes que usam MS-OFBA, você deve adicionar uma terceira parte confiável para o aplicativo para o serviço de Federação. Dependendo do aplicativo, você pode usar autenticação baseada em declarações ou autenticação integrada do Windows. Portanto, você deve adicionar o objeto de confiança de terceira parte confiável relevante, dependendo do aplicativo.  
  
Para permitir que o Proxy de aplicativo Web para executar logon único (SSO) e execute a delegação de credenciais usando o Kerberos, a delegação restrita, o Proxy de aplicativo Web server deve ser associado a um domínio. Ver [planejar o Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Não há etapas de planejamento adicionais se o aplicativo usar autenticação baseada em declarações. Se o aplicativo usou a autenticação do Windows integrada, consulte [publicar um aplicativo do Windows integrada com base em autenticado para clientes de navegador da Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2).  
  
O fluxo de autenticação para clientes que usam o protocolo MS-OFBA usando a autenticação baseada em declarações é descrito abaixo. A autenticação para esse cenário pode usar o token do aplicativo na URL, ou no corpo.  
  
1.  O usuário está trabalhando em um programa do Office e, na lista **Documentos Recentes**, abre um arquivo em um site do SharePoint.  
  
2.  O programa do Office mostra uma janela com um controle de navegador que exige que o usuário insira credenciais.  
  
    > [!NOTE]  
    > Em alguns casos, a janela pode não aparecer porque o cliente já está autenticado.  
  
3.  Proxy de aplicativo Web redireciona a solicitação para o servidor do AD FS, que executa a autenticação.  
  
4.  Servidor do AD FS redireciona a solicitação de volta para o Proxy de aplicativo Web. A solicitação agora contém o token de borda.  
  
5.  Servidor do AD FS adiciona um cookie de (SSO) logon único para a solicitação porque o usuário já executou a autenticação no servidor do AD FS.  
  
6.  Proxy de aplicativo Web valida o token e encaminha a solicitação para o servidor de back-end.  
  
7.  O servidor de back-end redireciona a solicitação para o servidor do AD FS para obter um token de segurança do aplicativo.  
  
8.  A solicitação é redirecionada ao servidor de back-end. A solicitação agora contém o token de aplicativo e o cookie de SSO. O usuário tem permissão para acessar ao site do SharePoint e não é necessário que ele insira um nome de usuário ou uma senha para exibir o arquivo.  
  
As etapas para publicar um aplicativo que usa o MS-OFBA são idênticas às etapas de um aplicativo baseado em declarações ou um aplicativo baseada em declarações. Para aplicativos baseados em declarações, consulte [publicar um aplicativo baseado em declarações para clientes de navegador da Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1), para aplicativos não baseados em declarações, consulte [publicar um aplicativo do Windows integrada com base em autenticado para Web Clientes de navegador](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2). Proxy de aplicativo Web automaticamente detecta o cliente e autenticará o usuário conforme necessário.  
  
## <a name="publish-an-application-that-uses-http-basic"></a>Publicar um aplicativo que usa básico de HTTP  

HTTP básico é o protocolo de autorização usado por vários protocolos, para conectar clientes avançados, incluindo smartphones, com sua caixa de correio do Exchange. Para obter mais informações sobre HTTP básica, consulte [RFC 2617](https://www.ietf.org/rfc/rfc2617.txt). Proxy de aplicativo Web tradicionalmente interage com o AD FS usando redirecionamentos; a maioria dos clientes avançados não dão suporte a cookies ou gerenciamento de estado. Dessa forma o Proxy de aplicativo Web permite que o aplicativo HTTP receber uma não-declarações terceira parte confiável para o aplicativo para o serviço de Federação. Ver [planejar o Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
O fluxo de autenticação para clientes que usam HTTP básica é descrito abaixo e neste diagrama:  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)  
  
1.  O usuário tenta acessar um aplicativo web publicado um cliente do telefone.  
  
2.  O aplicativo envia uma solicitação HTTPS para a URL publicada pelo Proxy de aplicativo Web.  
  
3.  Se a solicitação não contém as credenciais, o Proxy de aplicativo Web retorna uma resposta HTTP 401 para o aplicativo que contém a URL do servidor do AD FS de autenticação.  
  
4.  O usuário envia a solicitação HTTPS para o aplicativo novamente com a autorização definida como Basic e nome de usuário e de Base 64 criptografada senha do usuário no www-cabeçalho de solicitação de autenticação.  
  
5.  Porque o dispositivo não pode ser redirecionado para o AD FS, Proxy de aplicativo Web envia uma solicitação de autenticação para AD FS com as credenciais que ele tenha incluindo o nome de usuário e senha. O token é adquirido em nome do dispositivo.  
  
6.  Para minimizar o número de solicitações enviadas para o AD FS, Proxy de aplicativo Web, valida solicitações de cliente subsequentes usando tokens em cache para desde que o token é válido. Periodicamente, o Proxy de aplicativo Web limpa o cache. Você pode exibir o tamanho do cache usando o contador de desempenho.  
  
7.  Se o token for válido, o Proxy de aplicativo Web encaminha a solicitação para o servidor de back-end e o usuário recebe acesso ao aplicativo web publicado.  
  
O procedimento a seguir explica como publicar aplicativos básicos de HTTP.  
  
#### <a name="to-publish-an-http-basic-application"></a>Para publicar um aplicativo básico de HTTP  
  
1.  No servidor de Proxy de aplicativo Web, no console de gerenciamento de acesso remoto, nos **navegação** painel, clique em **Proxy de aplicativo Web**e, em seguida, no **tarefas** painel, clique em **Publicar**.  
  
2.  No **Assistente para Publicar Novo Aplicativo**, na página de **Boas-vindas**, clique em **Avançar**.  
  
3.  No **pré-autenticação** , clique em **serviços de Federação do Active Directory (AD FS)** e, em seguida, clique em **próxima**.  
  
4.  Sobre o **clientes com suporte** página, selecione **HTTP básica** e, em seguida, clique em **próxima**.  
  
    Se você quiser habilitar o acesso somente de dispositivos ingressados no local para o Exchange, selecione a **dispositivos adicionados ao habilitar o acesso somente para o local de trabalho** caixa. Para obter mais informações, consulte [ingresse no local de trabalho de qualquer dispositivo para SSO e contínuo segundo fator de autenticação em aplicativos da empresa](https://technet.microsoft.com/library/dn280945.aspx).  
  
5.  Na página **Terceira Parte Confiável**, na lista de terceiras partes confiáveis, selecione a terceira parte confiável para o aplicativo que pretende publicar e clique em **Avançar**. Observe que essa lista contém apenas terceiras partes confiáveis em declarações.  
  
6.  Na página **Configurações de publicação** , faça o seguinte e clique em **Avançar**:  
  
    -   Na caixa **Nome**, digite um nome amigável para o aplicativo.  
  
        Esse nome é usado apenas na lista de aplicativos publicados no console de Gerenciamento de Acesso Remoto.  
  
    -   No **URL externa** , digite a URL externa para esse aplicativo; por exemplo, mail.contoso.com  
  
    -   Na lista **Certificado externo**, selecione um certificado cujo assunto abranja a URL externa.  
  
    -   Na caixa **URL do servidor de back-end**, digite a URL do servidor de back-end. Observe que esse valor é automaticamente inserido quando você insere a URL externa e você deve alterá-lo somente se a URL do servidor de back-end for diferente; Por exemplo, mail.contoso.com.  
  
7.  Na página **Confirmação** , examine as configurações e clique em **Publicar**. É possível copiar o comando do PowerShell para configurar aplicativos publicados adicionais.  
  
8.  Na página **Resultados**, verifique se o aplicativo foi publicado com êxito e clique em **Fechar**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Este script do Windows PowerShell permite que a pré-autenticação para todos os dispositivos, não apenas os dispositivos ingressados no local.  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
O seguinte pré-autentica somente dispositivos ingressados no local de trabalho:  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -EnableHTTPRedirect:$true   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
## <a name="BKMK_1.4"></a>Publicar um aplicativo que usa OAuth2, como um aplicativo da Microsoft Store  
Para publicar um aplicativo para aplicativos da Microsoft Store, você deve adicionar uma terceira parte confiável para o aplicativo para o serviço de Federação.  
  
Para permitir que o Proxy de aplicativo Web para executar logon único (SSO) e execute a delegação de credenciais usando o Kerberos, a delegação restrita, o Proxy de aplicativo Web server deve ser associado a um domínio. Ver [planejar o Active Directory](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
> [!NOTE]  
> Proxy de aplicativo Web dá suporte à publicação apenas para aplicativos da Microsoft Store que usam o protocolo OAuth 2.0.  
  
No console de gerenciamento do AD FS, certifique-se de que o ponto de extremidade OAuth está habilitado para proxy. Para verificar se o ponto de extremidade OAuth está habilitado para proxy, abra o console de Gerenciamento do AD FS, expanda **Serviço**, clique em **Pontos de Extremidade**, na lista **Pontos de Extremidade** , localize o ponto de extremidade OAuth e verifique se o valor na coluna **Habilitado para Proxy** é **Sim**.  
  
O fluxo de autenticação para clientes que usam aplicativos da Microsoft Store é descrito abaixo:  
  
> [!NOTE]  
> O Proxy de aplicativo Web redireciona para o servidor do AD FS para autenticação. Porque os aplicativos da Microsoft Store não dão suporte a redirecionamentos, se você usar aplicativos da Microsoft Store, é necessário definir a URL do servidor do AD FS usando o cmdlet Set-WebApplicationProxyConfiguration e o parâmetro OAuthAuthenticationURL.  
>   
> Aplicativos da Microsoft Store podem ser publicados apenas usando o Windows PowerShell.  
  
1.  O cliente tenta acessar um aplicativo web publicado usando um aplicativo da Microsoft Store.  
  
2.  O aplicativo envia uma solicitação HTTPS para a URL publicada pelo Proxy de aplicativo Web.  
  
3.  Proxy de aplicativo Web retorna uma resposta HTTP 401 para o aplicativo que contém a URL do servidor do AD FS de autenticação. Esse processo é conhecido como "descoberta".  
  
    > [!NOTE]  
    > Se o aplicativo souber a URL do servidor do AD FS de autenticação e já tiver um token de combinação que contém o token OAuth e o token de borda, as etapas 2 e 3 são ignoradas neste fluxo de autenticação.  
  
4.  O aplicativo envia uma solicitação HTTPS para o servidor do AD FS.  
  
5.  O aplicativo usa o agente de autenticação da web para gerar uma caixa de diálogo na qual o usuário insere credenciais para autenticar o servidor do AD FS. Para obter informações sobre o agente de autenticação da Web, consulte [Agente de autenticação da Web](https://msdn.microsoft.com/library/windows/apps/hh750287.aspx).  
  
6.  Após a autenticação bem-sucedida, o servidor do AD FS cria um token de combinação que contém o token OAuth e o token de borda e envia o token para o aplicativo.  
  
7.  O aplicativo envia uma solicitação HTTPS que contém o token de combinação para a URL publicada pelo Proxy de aplicativo Web.  
  
8.  Proxy de aplicativo Web divide o token de combinação em suas duas partes e valida o token de borda.  
  
9. Se o token de borda for válido, o Proxy de aplicativo Web encaminha a solicitação para o servidor de back-end apenas com o token OAuth. O usuário tem permissão para acessar o aplicativo Web publicado.  
  
Este procedimento descreve como publicar um aplicativo para OAuth2. Esse tipo de aplicativo pode ser publicado apenas usando o Windows PowerShell. Antes de começar, certifique-se de que você tenha feito o seguinte:  
  
-   Criou uma terceira parte confiável para o aplicativo no console de gerenciamento do AD FS.  
  
-   Feita se o ponto de extremidade OAuth está habilitado para proxy no console do gerenciamento do AD FS e anote o caminho da URL.  
  
-   Verificado se um certificado no servidor de Proxy de aplicativo Web é adequado para o aplicativo que você deseja publicar.  
  
#### <a name="to-publish-an-oauth2-app"></a>Para publicar um aplicativo de OAuth2  
  
1.  No servidor de Proxy de aplicativo Web, no console de gerenciamento de acesso remoto, nos **navegação** painel, clique em **Proxy de aplicativo Web**e, em seguida, no **tarefas** painel, clique em **Publicar**.  
  
2.  No **Assistente para Publicar Novo Aplicativo**, na página de **Boas-vindas**, clique em **Avançar**.  
  
3.  No **pré-autenticação** , clique em **serviços de Federação do Active Directory (AD FS)** e, em seguida, clique em **próxima**.  
  
4.  Sobre o **clientes com suporte** página, selecione **OAuth2** e, em seguida, clique em **próxima**.  
  
5.  Na página **Terceira Parte Confiável**, na lista de terceiras partes confiáveis, selecione a terceira parte confiável para o aplicativo que pretende publicar e clique em **Avançar**.  
  
6.  Na página **Configurações de publicação** , faça o seguinte e clique em **Avançar**:  
  
    -   Na caixa **Nome**, digite um nome amigável para o aplicativo.  
  
        Esse nome é usado apenas na lista de aplicativos publicados no console de Gerenciamento de Acesso Remoto.  
  
    -   Na caixa **URL Externa**, insira a URL externa para esse aplicativo; por exemplo, https://server1.contoso.com/app1/.  
  
    -   Na lista **Certificado externo**, selecione um certificado cujo assunto abranja a URL externa.  
  
        Para certificar-se de que os usuários podem acessar seu aplicativo, mesmo se eles não digitar a URL HTTPS, selecione a **habilitar o redirecionamento de HTTP para HTTPS** caixa.  
  
    -   Na caixa **URL do servidor de back-end**, digite a URL do servidor de back-end. Observe que esse valor é automaticamente inserido quando você insere a URL externa e você deve alterá-lo somente se a URL do servidor de back-end for diferente; Por exemplo, https://sp/app1/.  
  
        > [!NOTE]  
        > Proxy de aplicativo Web pode converter nomes de host em URLs, mas não pode converter nomes de caminho. Portanto, você pode inserir nomes de host diferentes, mas deve inserir o mesmo nome de caminho. Por exemplo, você pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL do servidor de back-end de https://app-server/app1/. No entanto, você não pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL do servidor de back-end de https://apps.contoso.com/internal-app1/.  
  
7.  Na página **Confirmação** , examine as configurações e clique em **Publicar**. É possível copiar o comando do PowerShell para configurar aplicativos publicados adicionais.  
  
8.  Na página **Resultados**, verifique se o aplicativo foi publicado com êxito e clique em **Fechar**.  
  
Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
Para definir a URL de autenticação OAuth para um servidor de federação, endereço de fs.contoso.com e um caminho de URL de /adfs/oauth2 /:  
  
```  
Set-WebApplicationProxyConfiguration -OAuthAuthenticationURL 'https://fs.contoso.com/adfs/oauth2/'  
```  
  
Para publicar o aplicativo:  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://storeapp.contoso.com/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://storeapp.contoso.com/'  
    -Name 'Microsoft Store app Server'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'Store_app_Relying_Party'  
    -UseOAuthAuthentication  
```  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Solução de problemas de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn770156.aspx)  
  
-   [Publicar aplicativos por meio do Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383659.aspx)  
  
-   [Planejando publicar aplicativos usando o Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383650.aspx)  
  
-   [Guia de instruções passo a passo de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn280944.aspx)  
  
-   [Cmdlets do Proxy de aplicativo Web no Windows PowerShell](https://technet.microsoft.com/library/dn283404.aspx)  
  
-   [Add-WebApplicationProxyApplication](https://technet.microsoft.com/library/dn283409.aspx)  
  
-   [Set-WebApplicationProxyConfiguration](https://technet.microsoft.com/library/dn283406.aspx)  
  


