---
ms.assetid: 5f733510-c96e-4d3a-85d2-4407de95926e
title: Publicar aplicativos usando a Pré-autenticação do AD FS
ms.author: kgremban
author: eross-msft
ms.date: 07/13/2016
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: 97bfae42c873ecf7196138920a21d96714239da9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818699"
---
# <a name="publishing-applications-using-ad-fs-preauthentication"></a>Publicar aplicativos usando a Pré-autenticação do AD FS

>Aplica-se a: Windows Server 2016

**Esse conteúdo é relevante para a versão local do proxy de aplicativo Web. Para habilitar o acesso seguro a aplicativos locais na nuvem, consulte o conteúdo de [proxy de aplicativo do AD do Azure](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
Este tópico descreve como publicar aplicativos por meio do proxy de aplicativo Web usando a pré-autenticação de Serviços de Federação do Active Directory (AD FS) (AD FS).  
  
Para todos os tipos de aplicativos que você pode publicar usando AD FS pré-autenticação, você deve adicionar uma AD FS terceira parte confiável à Serviço de Federação.  
  
O fluxo de pré-autenticação AD FS geral é o seguinte:  
  
> [!NOTE]  
> Esse fluxo de autenticação não é aplicável para clientes que usam aplicativos Microsoft Store.  
  
1.  O dispositivo cliente tenta acessar um aplicativo Web publicado em uma URL de recurso específica; por exemplo https://app1.contoso.com/.  
  
    A URL do recurso é um endereço público no qual o proxy de aplicativo Web escuta solicitações HTTPS de entrada.  
  
    Se o redirecionamento de HTTP para HTTPS estiver habilitado, o proxy de aplicativo Web também escutará solicitações HTTP de entrada.  
  
2.  O proxy de aplicativo Web redireciona a solicitação HTTPS para o servidor de AD FS com parâmetros codificados de URL, incluindo a URL de recurso e o appRealm (um identificador de terceira parte confiável).  
  
    O usuário é autenticado usando o método de autenticação exigido pelo servidor de AD FS; por exemplo, nome de usuário e senha, autenticação de dois fatores com uma senha de uso único e assim por diante.  
  
3.  Depois que o usuário é autenticado, o servidor de AD FS emite um token de segurança, o ' token de borda ', que contém as informações a seguir e redireciona a solicitação HTTPS de volta para o servidor proxy de aplicativo Web:  
  
    -   O identificador de recurso que o usuário tentou acessar.  
  
    -   A identidade do usuário como um nome principal de usuário (UPN).  
  
    -   A expiração da aprovação de concessão de acesso; isto é, é concedido acesso ao usuário por um período de tempo limitado, após o qual é exigido que eles se autentiquem novamente.  
  
    -   Assinatura das informações no token de borda.  
  
4.  O proxy de aplicativo Web recebe a solicitação HTTPS redirecionada do servidor de AD FS com o token de borda e valida e usa o token da seguinte maneira:  
  
    -   Valida se a assinatura do token de borda é do serviço de Federação configurado na configuração do proxy de aplicativo Web.  
  
    -   Valida que o token foi emitido para o aplicativo correto.  
  
    -   Valida que o token não expirou.  
  
    -   Usa a identidade do usuário quando necessário; por exemplo, para obter um tíquete de Kerberos se o servidor de back-end estiver configurado para usar a autenticação integrada do Windows.  
  
5.  Se o token de borda for válido, o proxy de aplicativo Web encaminhará a solicitação HTTPS para o aplicativo Web publicado usando HTTP ou HTTPS.  
  
6.  O cliente agora tem acesso ao aplicativo Web publicado; no entanto, o aplicativo publicado pode estar configurado para exigir que o usuário execute autenticação adicional. Se, por exemplo, o aplicativo Web publicado for um site do SharePoint e não exigir autenticação adicional, o usuário verá o site do SharePoint no navegador.  
  
7.  O proxy de aplicativo Web salva um cookie no dispositivo cliente. O cookie é usado pelo proxy de aplicativo Web para identificar que essa sessão já foi autenticada e que nenhuma pré-autenticação adicional é necessária.  
  
> [!IMPORTANT]  
> Ao configurar a URL externa e a URL do servidor de back-end, você deve incluir o nome de domínio totalmente qualificado (FQDN), e não um endereço IP.  
  
> [!NOTE]  
> Este tópico inclui cmdlets de exemplo do Windows PowerShell que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="publish-a-claims-based-application-for-web-browser-clients"></a><a name="BKMK_1.1"></a>Publicar um aplicativo baseado em declarações para clientes de navegador da Web  
Para publicar um aplicativo que usa declarações para autenticação, você deve adicionar ao Serviço de Federação um objeto de confiança de terceira parte confiável para o aplicativo.  
  
Ao publicar aplicativos baseados em declarações e acessar o aplicativo de um navegador, o fluxo geral de autenticação será o seguinte:  
  
1.  O cliente tenta acessar um aplicativo baseado em declarações usando um navegador da Web; por exemplo, https://appserver.contoso.com/claimapp/.  
  
2.  O navegador da Web envia uma solicitação HTTPS para o servidor proxy de aplicativo Web que redireciona a solicitação para o servidor de AD FS.  
  
3.  O servidor de AD FS autentica o usuário e o dispositivo e redireciona a solicitação de volta para o proxy de aplicativo Web. A solicitação agora contém o token de borda. O AD FS Server adiciona um cookie de logon único (SSO) à solicitação porque o usuário já realizou a autenticação no servidor AD FS.  
  
4.  O proxy de aplicativo Web valida o token, adiciona seu próprio cookie e encaminha a solicitação para o servidor de back-end.  
  
5.  O servidor back-end redireciona a solicitação para o servidor de AD FS para obter o token de segurança do aplicativo.  
  
6.  A solicitação é redirecionada para o servidor de back-end pelo servidor de AD FS. A solicitação agora contém o token de aplicativo e o cookie de SSO. O usuário recebe permissão para acessar o aplicativo e não é necessário que digite um nome de usuário ou uma senha.  
  
Este procedimento descreve como publicar um aplicativo baseado em declarações, como um site do SharePoint, que será acessado por clientes do navegador da Web. Antes de começar, certifique-se de que você tenha feito o seguinte:  
  
-   Criou uma relação de confiança de terceira parte confiável para o aplicativo no console de gerenciamento de AD FS.  
  
-   Verificado se um certificado no servidor proxy de aplicativo Web é adequado para o aplicativo que você deseja publicar.  
  

  
#### <a name="to-publish-a-claims-based-application"></a>Para publicar um aplicativo baseado em declarações  
  
1.  No servidor proxy de aplicativo Web, no console de gerenciamento de acesso remoto, no painel de **navegação** , clique em **proxy de aplicativo Web**e, no painel **tarefas** , clique em **publicar**.  
  
2.  No **Assistente para Publicar Novo Aplicativo**, na página de **Boas-vindas**, clique em **Avançar**.  
  
3.  Na página **pré-autenticação** , clique em **serviços de Federação do Active Directory (AD FS) (AD FS)** e, em seguida, clique em **Avançar**.  
  
4.  Na página **Clientes com Suporte** selecione **Web e MSOFBA**, e, em seguida, clique em **Avançar**.  
  
5.  Na página **Terceira Parte Confiável**, na lista de terceiras partes confiáveis, selecione a terceira parte confiável para o aplicativo que pretende publicar e clique em **Avançar**.  
  
6.  Na página **Configurações de publicação** , faça o seguinte e clique em **Avançar**:  
  
    -   Na caixa **Nome**, digite um nome amigável para o aplicativo.  
  
        Esse nome é usado apenas na lista de aplicativos publicados no console de Gerenciamento de Acesso Remoto.  
  
    -   Na caixa **URL Externa**, insira a URL externa para esse aplicativo; por exemplo, https://sp.contoso.com/app1/.  
  
    -   Na lista **Certificado externo**, selecione um certificado cujo assunto abranja a URL externa.  
  
    -   Na caixa **URL do servidor de back-end**, digite a URL do servidor de back-end. Observe que esse valor é inserido automaticamente quando você insere a URL externa e você deve alterá-la somente se a URL do servidor de back-end for diferente; por exemplo, https://sp/app1/.  
  
        > [!NOTE]  
        > O proxy de aplicativo Web pode converter nomes de host em URLs, mas não pode converter nomes de caminho. Portanto, você pode inserir nomes de host diferentes, mas deve inserir o mesmo nome de caminho. Por exemplo, você pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL de servidor de back-end do https://app-server/app1/. No entanto, você não pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL de servidor de back-end do https://apps.contoso.com/internal-app1/.  
  
7.  Na página **Confirmação** , examine as configurações e clique em **Publicar**. É possível copiar o comando do PowerShell para configurar aplicativos publicados adicionais.  
  
8.  Na página **Resultados**, verifique se o aplicativo foi publicado com êxito e clique em **Fechar**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif) ***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
```  
Add-WebApplicationProxyApplication  
    -BackendServerURL 'https://sp.contoso.com/app1/'  
    -ExternalCertificateThumbprint '1a2b3c4d5e6f1a2b3c4d5e6f1a2b3c4d5e6f1a2b'  
    -ExternalURL 'https://sp.contoso.com/app1/'  
    -Name 'SP'  
    -ExternalPreAuthentication ADFS  
    -ADFSRelyingPartyName 'SP_Relying_Party'  
```  
  
## <a name="publish-an-integrated-windows-authenticated-based-application-for-web-browser-clients"></a><a name="BKMK_1.2"></a>Publicar um aplicativo integrado baseado no Windows autenticado para clientes de navegador da Web  
O proxy de aplicativo Web pode ser usado para publicar aplicativos que usam a autenticação integrada do Windows; ou seja, o proxy de aplicativo Web executa a pré-autenticação conforme necessário e, em seguida, pode executar o SSO para o aplicativo publicado que usa a autenticação integrada do Windows. Para publicar um aplicativo que usa autenticação integrada do Windows, você deve adicionar ao Serviço de Federação um objeto de confiança de terceira parte confiável sem reconhecimento de declarações para o aplicativo.  
  
Para permitir que o proxy de aplicativo Web execute SSO (logon único) e execute a delegação de credenciais usando a delegação restrita de Kerberos, o servidor proxy de aplicativo Web deve ser Unido a um domínio. Consulte [Active Directory de plano](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Para permitir que os usuários acessem aplicativos que usam a autenticação integrada do Windows, o servidor proxy de aplicativo Web deve ser capaz de fornecer delegação para os usuários para o aplicativo publicado. Você pode fazer isso no controlador de domínio de qualquer aplicativo. Você também pode fazer isso no servidor de back-end se ele estiver em execução no Windows Server 2012 R2 ou no Windows Server 2012. Para obter mais informações, consulte [Novidades na autenticação Kerberos](https://technet.microsoft.com/library/hh831747.aspx).  
  
Para obter uma explicação de como configurar o proxy de aplicativo Web para publicar um aplicativo usando a autenticação integrada do Windows, consulte [configurar um site para usar a autenticação integrada do Windows](https://technet.microsoft.com/library/dn280943.aspx#BKMK_3).  
  
Ao usar a autenticação integrada do Windows para servidores de back-end, a autenticação entre o proxy de aplicativo Web e o aplicativo publicado não é baseada em declarações. em vez disso, ele usa a delegação restrita de Kerberos para autenticar os usuários finais no aplicativo. O fluxo geral é descrito abaixo:  
  
1.  O cliente tenta acessar um aplicativo não baseado em declarações usando um navegador da Web; por exemplo, https://appserver.contoso.com/nonclaimapp/.  
  
2.  O navegador da Web envia uma solicitação HTTPS para o servidor proxy de aplicativo Web que redireciona a solicitação para o servidor de AD FS.  
  
3.  O servidor de AD FS autentica o usuário e redireciona a solicitação de volta para o proxy de aplicativo Web. A solicitação agora contém o token de borda.  
  
4.  O proxy de aplicativo Web valida o token.  
  
5.  Se o token for válido, o proxy de aplicativo Web obterá um tíquete Kerberos do controlador de domínio em nome do usuário.  
  
6.  O proxy de aplicativo Web adiciona o tíquete Kerberos à solicitação como parte do token de SPNEGO (mecanismo de negociação de GSS-API) simples e protegido e encaminha a solicitação para o servidor de back-end. A solicitação contém o tíquete de Kerberos; portanto, o usuário ter permissão para acessar o aplicativo sem necessidade de autenticação adicional.  
  
Este procedimento descreve como publicar um aplicativo que usa a autenticação integrada do Windows, como o Outlook Web App, que será acessado por clientes do navegador da Web. Antes de começar, certifique-se de que você tenha feito o seguinte:  
  
-   Foi criada uma relação de confiança de terceira parte confiável sem reconhecimento de declaração para o aplicativo no console de gerenciamento de AD FS.  
  
-   Configurado o servidor de back-end para dar suporte à delegação restrita de Kerberos no controlador de domínio ou usando o cmdlet Set-ADUser com o parâmetro -PrincipalsAllowedToDelegateToAccount. Observe que, se o servidor back-end estiver em execução no Windows Server 2012 R2 ou no Windows Server 2012, você também poderá executar esse comando do PowerShell no servidor de back-end.  
  
-   Certifique-se de que os servidores proxy de aplicativo Web estejam configurados para delegação para os nomes de entidade de serviço dos servidores de back-end.  
  
-   Verificado se um certificado no servidor proxy de aplicativo Web é adequado para o aplicativo que você deseja publicar.  
  
 
  
#### <a name="to-publish-a-non-claims-based-application"></a>Para publicar um aplicativo que não é baseado em declarações  
  
1.  No servidor proxy de aplicativo Web, no console de gerenciamento de acesso remoto, no painel de **navegação** , clique em **proxy de aplicativo Web**e, no painel **tarefas** , clique em **publicar**.  
  
2.  No **Assistente para Publicar Novo Aplicativo**, na página de **Boas-vindas**, clique em **Avançar**.  
  
3.  Na página **pré-autenticação** , clique em **serviços de Federação do Active Directory (AD FS) (AD FS)** e, em seguida, clique em **Avançar**.  
  
4.  Na página **Clientes com Suporte** selecione **Web e MSOFBA**, e, em seguida, clique em **Avançar**.  
  
5.  Na página **Terceira Parte Confiável**, na lista de terceiras partes confiáveis, selecione a terceira parte confiável para o aplicativo que pretende publicar e clique em **Avançar**.  
  
6.  Na página **Configurações de publicação** , faça o seguinte e clique em **Avançar**:  
  
    -   Na caixa **Nome**, digite um nome amigável para o aplicativo.  
  
        Esse nome é usado apenas na lista de aplicativos publicados no console de Gerenciamento de Acesso Remoto.  
  
    -   Na caixa **URL Externa**, insira a URL externa para esse aplicativo; por exemplo, https://owa.contoso.com/.  
  
    -   Na lista **Certificado externo**, selecione um certificado cujo assunto abranja a URL externa.  
  
    -   Na caixa **URL do servidor de back-end**, digite a URL do servidor de back-end. Observe que esse valor é inserido automaticamente quando você insere a URL externa e você deve alterá-la somente se a URL do servidor de back-end for diferente; por exemplo, https://owa/.  
  
        > [!NOTE]  
        > O proxy de aplicativo Web pode converter nomes de host em URLs, mas não pode converter nomes de caminho. Portanto, você pode inserir nomes de host diferentes, mas deve inserir o mesmo nome de caminho. Por exemplo, você pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL de servidor de back-end do https://app-server/app1/. No entanto, você não pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL de servidor de back-end do https://apps.contoso.com/internal-app1/.  
  
    -   Na caixa **SPN do servidor de back-end**, insira o nome da entidade de serviço para o servidor de back-end; por exemplo, HTTP/owa.contoso.com.  
  
7.  Na página **Confirmação** , examine as configurações e clique em **Publicar**. É possível copiar o comando do PowerShell para configurar aplicativos publicados adicionais.  
  
8.  Na página **Resultados**, verifique se o aplicativo foi publicado com êxito e clique em **Fechar**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif) ***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
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
  
## <a name="publish-an-application-that-uses-ms-ofba"></a><a name="BKMK_1.3"></a>Publicar um aplicativo que usa MS-OFBA  
O proxy de aplicativo Web dá suporte ao acesso de clientes Microsoft Office, como o Microsoft Word, que acessam documentos e dados em servidores de back-end. A única diferença entre esses aplicativos e um navegador padrão é que o redirecionamento para o STS não é feito por meio do redirecionamento de HTTP regular, mas com cabeçalhos especiais do MS-OFBA, conforme especificado em: [https://msdn.microsoft.com/library/dd773463(v=office.12).aspx](https://msdn.microsoft.com/library/dd773463(v=office.12).aspx). O aplicativo de back-end pode ser declarações ou IWA.   
Para publicar um aplicativo para clientes que usam o MS-OFBA, você deve adicionar um confiança de terceira parte confiável para o aplicativo ao Serviço de Federação. Dependendo do aplicativo, você pode usar autenticação baseada em declarações ou autenticação integrada do Windows. Portanto, você deve adicionar o objeto de confiança de terceira parte confiável relevante, dependendo do aplicativo.  
  
Para permitir que o proxy de aplicativo Web execute SSO (logon único) e execute a delegação de credenciais usando a delegação restrita de Kerberos, o servidor proxy de aplicativo Web deve ser Unido a um domínio. Consulte [Active Directory de plano](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
Não há etapas de planejamento adicionais se o aplicativo usar autenticação baseada em declarações. Se o aplicativo usava a autenticação integrada do Windows, consulte [publicar um aplicativo integrado baseado no Windows autenticado para clientes de navegador da Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2).  
  
O fluxo de autenticação para clientes que usam o protocolo MS-OFBA usando a autenticação baseada em declarações é descrito abaixo. A autenticação para esse cenário pode usar o token do aplicativo na URL, ou no corpo.  
  
1.  O usuário está trabalhando em um programa do Office e, na lista **Documentos Recentes**, abre um arquivo em um site do SharePoint.  
  
2.  O programa do Office mostra uma janela com um controle de navegador que exige que o usuário insira credenciais.  
  
    > [!NOTE]  
    > Em alguns casos, a janela pode não aparecer porque o cliente já está autenticado.  
  
3.  O proxy de aplicativo Web redireciona a solicitação para o servidor de AD FS, que executa a autenticação.  
  
4.  O servidor de AD FS redireciona a solicitação de volta para o proxy de aplicativo Web. A solicitação agora contém o token de borda.  
  
5.  O AD FS Server adiciona um cookie de logon único (SSO) à solicitação porque o usuário já realizou a autenticação no servidor AD FS.  
  
6.  O proxy de aplicativo Web valida o token e encaminha a solicitação para o servidor de back-end.  
  
7.  O servidor back-end redireciona a solicitação para o servidor de AD FS para obter o token de segurança do aplicativo.  
  
8.  A solicitação é redirecionada ao servidor de back-end. A solicitação agora contém o token de aplicativo e o cookie de SSO. O usuário tem permissão para acessar ao site do SharePoint e não é necessário que ele insira um nome de usuário ou uma senha para exibir o arquivo.  
  
As etapas para publicar um aplicativo que usa MS-OFBA são idênticas às etapas para um aplicativo baseado em declarações ou um aplicativo não baseado em declarações. Para aplicativos baseados em declarações, consulte [publicar um aplicativo baseado em declarações para clientes de navegador da Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.1), para aplicativos não baseados em declarações, consulte [publicar um aplicativo integrado baseado no Windows autenticado para clientes de navegador da Web](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md#BKMK_1.2). O proxy de aplicativo Web detecta automaticamente o cliente e autentica o usuário conforme necessário.  
  
## <a name="publish-an-application-that-uses-http-basic"></a>Publicar um aplicativo que usa HTTP básico  

O HTTP básico é o protocolo de autorização usado por muitos protocolos, para conectar clientes avançados, incluindo smartphones, com sua caixa de correio do Exchange. Para obter mais informações sobre HTTP básico, consulte [RFC 2617](https://www.ietf.org/rfc/rfc2617.txt). O proxy de aplicativo Web tradicionalmente interage com AD FS usando redirecionamentos; Os clientes mais avançados não dão suporte ao gerenciamento de cookies ou de estado. Dessa forma, o proxy de aplicativo Web permite que o aplicativo HTTP receba uma relação de confiança de terceira parte confiável sem declarações para o aplicativo para a Serviço de Federação. Consulte [Active Directory de plano](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
O fluxo de autenticação para clientes que usam HTTP básico é descrito abaixo e neste diagrama:  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/WebApplicationProxy_httpBasicflow.png)  
  
1.  O usuário tenta acessar um aplicativo Web publicado como um cliente telefônico.  
  
2.  O aplicativo envia uma solicitação HTTPS para a URL publicada pelo proxy de aplicativo Web.  
  
3.  Se a solicitação não contiver credenciais, o proxy de aplicativo Web retornará uma resposta HTTP 401 para o aplicativo que contém a URL do servidor de AD FS de autenticação.  
  
4.  O usuário envia a solicitação HTTPS para o aplicativo novamente com a autorização definida como básica e o nome de usuário e a senha criptografada base 64 do usuário no cabeçalho de solicitação www-Authenticate.  
  
5.  Como o dispositivo não pode ser redirecionado para AD FS, o proxy de aplicativo Web envia uma solicitação de autenticação para AD FS com as credenciais que inclui o nome de usuário e a senha. O token é adquirido em nome do dispositivo.  
  
6.  Para minimizar o número de solicitações enviadas ao AD FS, o proxy de aplicativo Web, valida as solicitações de cliente subsequentes usando tokens em cache, desde que o token seja válido. O proxy de aplicativo Web limpa periodicamente o cache. Você pode exibir o tamanho do cache usando o contador de desempenho.  
  
7.  Se o token for válido, o proxy de aplicativo Web encaminha a solicitação para o servidor de back-end e o usuário recebe acesso ao aplicativo Web publicado.  
  
O procedimento a seguir explica como publicar aplicativos HTTP básicos.  
  
#### <a name="to-publish-an-http-basic-application"></a>Para publicar um aplicativo HTTP básico  
  
1.  No servidor proxy de aplicativo Web, no console de gerenciamento de acesso remoto, no painel de **navegação** , clique em **proxy de aplicativo Web**e, no painel **tarefas** , clique em **publicar**.  
  
2.  No **Assistente para Publicar Novo Aplicativo**, na página de **Boas-vindas**, clique em **Avançar**.  
  
3.  Na página **pré-autenticação** , clique em **serviços de Federação do Active Directory (AD FS) (AD FS)** e, em seguida, clique em **Avançar**.  
  
4.  Na página **clientes com suporte** , selecione **http básico** e clique em **Avançar**.  
  
    Se você quiser habilitar o acesso ao Exchange somente de dispositivos ingressados no local de trabalho, selecione a caixa **habilitar acesso somente para dispositivos ingressados no local de trabalho** . Para obter mais informações [, consulte ingressar no local de trabalho de qualquer dispositivo para SSO e autenticação de segundo fator direta entre aplicativos da empresa](https://technet.microsoft.com/library/dn280945.aspx).  
  
5.  Na página **Terceira Parte Confiável**, na lista de terceiras partes confiáveis, selecione a terceira parte confiável para o aplicativo que pretende publicar e clique em **Avançar**. Observe que essa lista contém apenas partes confiáveis de Claims.  
  
6.  Na página **Configurações de publicação** , faça o seguinte e clique em **Avançar**:  
  
    -   Na caixa **Nome**, digite um nome amigável para o aplicativo.  
  
        Esse nome é usado apenas na lista de aplicativos publicados no console de Gerenciamento de Acesso Remoto.  
  
    -   Na caixa **URL externa** , insira a URL externa para este aplicativo; por exemplo, mail.contoso.com  
  
    -   Na lista **Certificado externo**, selecione um certificado cujo assunto abranja a URL externa.  
  
    -   Na caixa **URL do servidor de back-end**, digite a URL do servidor de back-end. Observe que esse valor é inserido automaticamente quando você insere a URL externa e você deve alterá-la somente se a URL do servidor de back-end for diferente; por exemplo, mail.contoso.com.  
  
7.  Na página **Confirmação** , examine as configurações e clique em **Publicar**. É possível copiar o comando do PowerShell para configurar aplicativos publicados adicionais.  
  
8.  Na página **Resultados**, verifique se o aplicativo foi publicado com êxito e clique em **Fechar**.  
  
![](../../media/Publishing-Applications-using-AD-FS-Preauthentication/PowerShellLogoSmall.gif) ***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
Este script do Windows PowerShell habilita a pré-autenticação para todos os dispositivos, não apenas para dispositivos ingressados no local de trabalho.  
  
```  
Add-WebApplicationProxyApplication  
     -BackendServerUrl 'https://mail.contoso.com'   
     -ExternalCertificateThumbprint '697F4FF0B9947BB8203A96ED05A3021830638E50'   
     -ExternalUrl 'https://mail.contoso.com'   
     -Name 'Exchange'   
     -ExternalPreAuthentication ADFSforRichClients  
     -ADFSRelyingPartyName 'EAS_Relying_Party'  
```  
  
O seguinte autentica apenas os dispositivos ingressados no local de trabalho:  
  
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
  
## <a name="publish-an-application-that-uses-oauth2-such-as-a-microsoft-store-app"></a><a name="BKMK_1.4"></a>Publicar um aplicativo que usa OAuth2 como um aplicativo Microsoft Store  
Para publicar um aplicativo para Microsoft Store aplicativos, você deve adicionar um confiança de terceira parte confiável para o aplicativo ao Serviço de Federação.  
  
Para permitir que o proxy de aplicativo Web execute SSO (logon único) e execute a delegação de credenciais usando a delegação restrita de Kerberos, o servidor proxy de aplicativo Web deve ser Unido a um domínio. Consulte [Active Directory de plano](https://technet.microsoft.com/library/dn383648.aspx#BKMK_AD).  
  
> [!NOTE]  
> O proxy de aplicativo Web dá suporte à publicação somente para aplicativos Microsoft Store que usam o protocolo OAuth 2,0.  
  
No console de gerenciamento do AD FS, você deve verificar se o ponto de extremidade OAuth está habilitado para proxy. Para verificar se o ponto de extremidade OAuth está habilitado para proxy, abra o console de Gerenciamento do AD FS, expanda **Serviço**, clique em **Pontos de Extremidade**, na lista **Pontos de Extremidade**, localize o ponto de extremidade OAuth e verifique se o valor na coluna **Habilitado para Proxy** é **Sim**.  
  
O fluxo de autenticação para clientes que usam aplicativos Microsoft Store é descrito abaixo:  
  
> [!NOTE]  
> O proxy de aplicativo Web redireciona para o servidor de AD FS para autenticação. Como os aplicativos Microsoft Store não dão suporte a redirecionamentos, se você usar Microsoft Store aplicativos, será necessário definir a URL do servidor de AD FS usando o cmdlet Set-WebApplicationProxyConfiguration e o parâmetro OAuthAuthenticationURL.  
>   
> Microsoft Store aplicativos podem ser publicados somente usando o Windows PowerShell.  
  
1.  O cliente tenta acessar um aplicativo Web publicado usando um aplicativo Microsoft Store.  
  
2.  O aplicativo envia uma solicitação HTTPS para a URL publicada pelo proxy de aplicativo Web.  
  
3.  O proxy de aplicativo Web retorna uma resposta HTTP 401 ao aplicativo que contém a URL do servidor de AD FS de autenticação. Esse processo é conhecido como ' descoberta '.  
  
    > [!NOTE]  
    > Se o aplicativo souber a URL do servidor de AD FS de autenticação e já tiver um token de combinação que contém o token OAuth e o token de borda, as etapas 2 e 3 serão ignoradas nesse fluxo de autenticação.  
  
4.  O aplicativo envia uma solicitação HTTPS para o servidor de AD FS.  
  
5.  O aplicativo usa o agente de autenticação da Web para gerar uma caixa de diálogo na qual o usuário insere as credenciais para se autenticar no servidor de AD FS. Para obter informações sobre o agente de autenticação da Web, consulte [Agente de autenticação da Web](https://msdn.microsoft.com/library/windows/apps/hh750287.aspx).  
  
6.  Após a autenticação bem-sucedida, o servidor de AD FS cria um token de combinação que contém o token OAuth e o token de borda e envia o token para o aplicativo.  
  
7.  O aplicativo envia uma solicitação HTTPS contendo o token de combinação para a URL publicada pelo proxy de aplicativo Web.  
  
8.  O proxy de aplicativo Web divide o token de combinação em suas duas partes e valida o token de borda.  
  
9. Se o token de borda for válido, o proxy de aplicativo Web encaminhará a solicitação para o servidor de back-end apenas com o token OAuth. O usuário tem permissão para acessar o aplicativo Web publicado.  
  
Este procedimento descreve como publicar um aplicativo para OAuth2. Esse tipo de aplicativo pode ser publicado somente usando o Windows PowerShell. Antes de começar, certifique-se de que você tenha feito o seguinte:  
  
-   Criou uma relação de confiança de terceira parte confiável para o aplicativo no console de gerenciamento de AD FS.  
  
-   Certifique-se de que o ponto de extremidade OAuth seja habilitado para proxy no console de gerenciamento de AD FS e anote o caminho da URL.  
  
-   Verificado se um certificado no servidor proxy de aplicativo Web é adequado para o aplicativo que você deseja publicar.  
  
#### <a name="to-publish-an-oauth2-app"></a>Para publicar um aplicativo OAuth2  
  
1.  No servidor proxy de aplicativo Web, no console de gerenciamento de acesso remoto, no painel de **navegação** , clique em **proxy de aplicativo Web**e, no painel **tarefas** , clique em **publicar**.  
  
2.  No **Assistente para Publicar Novo Aplicativo**, na página de **Boas-vindas**, clique em **Avançar**.  
  
3.  Na página **pré-autenticação** , clique em **serviços de Federação do Active Directory (AD FS) (AD FS)** e, em seguida, clique em **Avançar**.  
  
4.  Na página **clientes com suporte** , selecione **OAuth2** e clique em **Avançar**.  
  
5.  Na página **Terceira Parte Confiável**, na lista de terceiras partes confiáveis, selecione a terceira parte confiável para o aplicativo que pretende publicar e clique em **Avançar**.  
  
6.  Na página **Configurações de publicação** , faça o seguinte e clique em **Avançar**:  
  
    -   Na caixa **Nome**, digite um nome amigável para o aplicativo.  
  
        Esse nome é usado apenas na lista de aplicativos publicados no console de Gerenciamento de Acesso Remoto.  
  
    -   Na caixa **URL Externa**, insira a URL externa para esse aplicativo; por exemplo, https://server1.contoso.com/app1/.  
  
    -   Na lista **Certificado externo**, selecione um certificado cujo assunto abranja a URL externa.  
  
        Para garantir que os usuários possam acessar seu aplicativo, mesmo que eles não digitem HTTPS na URL, selecione a caixa **habilitar redirecionamento de http para https** .  
  
    -   Na caixa **URL do servidor de back-end**, digite a URL do servidor de back-end. Observe que esse valor é inserido automaticamente quando você insere a URL externa e você deve alterá-la somente se a URL do servidor de back-end for diferente; por exemplo, https://sp/app1/.  
  
        > [!NOTE]  
        > O proxy de aplicativo Web pode converter nomes de host em URLs, mas não pode converter nomes de caminho. Portanto, você pode inserir nomes de host diferentes, mas deve inserir o mesmo nome de caminho. Por exemplo, você pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL de servidor de back-end do https://app-server/app1/. No entanto, você não pode inserir uma URL externa de https://apps.contoso.com/app1/ e uma URL de servidor de back-end do https://apps.contoso.com/internal-app1/.  
  
7.  Na página **Confirmação** , examine as configurações e clique em **Publicar**. É possível copiar o comando do PowerShell para configurar aplicativos publicados adicionais.  
  
8.  Na página **Resultados**, verifique se o aplicativo foi publicado com êxito e clique em **Fechar**.  
  
Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
Para definir a URL de autenticação OAuth para um endereço de servidor de Federação de fs.contoso.com e um caminho de URL de/ADFS/oauth2/:  
  
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
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Consulte também  
  
-   [Como solucionar problemas de proxy de aplicativo Web](https://technet.microsoft.com/library/dn770156.aspx)  
  
-   [Publicar aplicativos por meio do proxy de aplicativo Web](https://technet.microsoft.com/library/dn383659.aspx)  
  
-   [Planejando a publicação de aplicativos usando o proxy de aplicativo Web](https://technet.microsoft.com/library/dn383650.aspx)  
  
-   [Guia explicativo do proxy de aplicativo Web](https://technet.microsoft.com/library/dn280944.aspx)  
  
-   [Cmdlets de proxy de aplicativo Web no Windows PowerShell](https://technet.microsoft.com/library/dn283404.aspx)  
  
-   [Add-WebApplicationProxyApplication](https://technet.microsoft.com/library/dn283409.aspx)  
  
-   [Set-WebApplicationProxyConfiguration](https://technet.microsoft.com/library/dn283406.aspx)  
  


