---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: Publicar aplicativos com SharePoint, Exchange e RDG
description: ''
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: web-app-proxy
ms.openlocfilehash: 21ea6ecf717392027c1d00f818e230e2fe56a11d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826817"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>Publicar aplicativos com SharePoint, Exchange e RDG

>Aplica-se a: Windows Server 2016

**Este conteúdo é relevante para a versão local do Proxy de aplicativo Web. Para habilitar o acesso seguro a aplicativos locais pela nuvem, consulte o [conteúdo de Proxy de aplicativo do Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
Este tópico descreve as tarefas necessárias para publicar o servidor do SharePoint, Exchange Server ou o Gateway de área de trabalho remota (RDP) por meio do Proxy de aplicativo Web.  

>[!NOTE]
>Essas informações são fornecidas como-está.  Remote Desktop Services dá suporte a e recomenda o uso de [Proxy de aplicativo do Azure para fornecer acesso remoto seguro a aplicativos locais](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).
  
## <a name="BKMK_6.1"></a>Publish SharePoint Server  
Você pode publicar um site do SharePoint por meio do Proxy de aplicativo Web quando o site do SharePoint está configurado para autenticação baseada em declarações ou autenticação do Windows integrado. Se você quiser usar os serviços de Federação do Active Directory (AD FS) para pré-autenticação, você deve configurar uma terceira parte confiável usando um dos assistentes.  
  
-   Se o site do SharePoint usa a autenticação baseada em declarações, você deve usar o Assistente para Adicionar o Objeto de Confiança de Terceira Parte Confiável para configurar o objeto de confiança de terceira parte confiável para o aplicativo.  
  
-   Se o site do SharePoint usa a autenticação integrada do Windows, você deve usar o Assistente para Adicionar o Objeto de Confiança de Terceira Parte Confiável Não Baseado em Declarações para configurar o objeto de confiança de terceira parte confiável para o aplicativo. Você pode usar a IWA com um aplicativo Web baseado em declarações desde que configure o KDC.  
  
    Para permitir que os usuários se autentiquem usando a autenticação do Windows integrada, o servidor de Proxy de aplicativo Web deve ser ingressado em um domínio.  
  
    Você deve configurar o aplicativo para dar suporte à delegação restrita de Kerberos. Você pode fazer isso no controlador de domínio de qualquer aplicativo. Você também pode configurar o aplicativo diretamente no servidor de back-end se ele estiver em execução no Windows Server 2012 R2 ou Windows Server 2012. Para obter mais informações, consulte [Novidades na autenticação Kerberos](https://technet.microsoft.com/library/hh831747.aspx). Você também deve garantir que os servidores de Proxy de aplicativo Web são configurados para delegação aos nomes da entidade de serviço dos servidores de back-end. Para obter uma explicação de como configurar o Proxy de aplicativo Web para publicar um aplicativo usando a autenticação do Windows integrada, consulte [configurar um site para usar a autenticação do Windows integrada](assetId:///b0788958-627f-450f-877c-209b1bd0db52).  
  
Se seu site do SharePoint for configurado usando AAM (mapeamentos alternativos de acesso) ou coleções de sites com nome de host, você pode usar diferentes URLs de servidor externo e de back-end para publicar seu aplicativo. No entanto, se você não configurar seu site do SharePoint usando o AAM ou coleções de sites com nome de host, deve usar as mesmas URLs de servidor externo e de back-end.  
  
## <a name="BKMK_6.2"></a>Publicar o Exchange Server  
A tabela a seguir descreve os serviços do Exchange que você pode publicar por meio do Proxy de aplicativo Web e a pré-autenticação com suporte para esses serviços:  
  
|Serviço do Exchange|Pré-autenticação|Observações|  
|--------------------|-----------------------|---------|  
|Outlook Web App|-O AD FS usando a autenticação baseada em declarações<br />-Passagem<br />-O AD FS usando a autenticação baseada em declarações para locais Exchange 2013 Service Pack 1 (SP1)|Para saber mais, confira: [Usando a autenticação baseada em declarações do AD FS com o Outlook Web App e o EAC](https://go.microsoft.com/fwlink/?LinkId=393723)|  
|Painel de controle do Exchange|Passagem||  
|Outlook em Qualquer Lugar|Passagem|Você deve publicar as três URLs para o Outlook em Qualquer Lugar funcionar corretamente:<br /><br />-A URL de descoberta automática.<br />-O nome de host externo do Exchange Server; ou seja, a URL que está configurada para que os clientes se conectem.<br />-O FQDN interno do servidor Exchange.|  
|Exchange ActiveSync|Passagem<br/> AD FS usando o protocolo de autorização HTTP básico|| Para obter mais informações, consulte: https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/publishing-applications-using-ad-fs-preauthentication 
  
Para publicar o Outlook Web App usando a autenticação integrada do Windows, você deve usar o Assistente para Adicionar o Objeto de Confiança de Terceira Parte Confiável Não Baseado em Declarações para configurar o objeto de confiança de terceira parte confiável para o aplicativo.  
  
Para permitir que os usuários se autentiquem usando Kerberos delegação restrita do que servidor de Proxy de aplicativo Web deve ser associado a um domínio.  
  
Você deve configurar o aplicativo para dar suporte à autenticação Kerberos. Além disso você precisará registrar um nome de entidade de serviço (SPN) para a conta que o serviço web está sendo executado. Você pode fazer isso no controlador de domínio ou nos servidores de back-end. Em uma carga equilibrada ambiente do Exchange, isso exigiria usando a conta de serviço alternativo, consulte [Configurando a autenticação Kerberos para servidores de acesso para cliente com balanceamento de carga](https://technet.microsoft.com/library/ff808312(v=exchg.150).aspx)  
  
Você também pode configurar o aplicativo diretamente no servidor de back-end se ele estiver em execução no Windows Server 2012 R2 ou Windows Server 2012. Para obter mais informações, consulte [Novidades na autenticação Kerberos](https://technet.microsoft.com/library/hh831747.aspx). Você também deve garantir que os servidores de Proxy de aplicativo Web são configurados para delegação aos nomes da entidade de serviço dos servidores de back-end.  
  
## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>Publicando o Gateway de área de trabalho remota por meio do Proxy de aplicativo Web  
Se você quiser restringir o acesso ao seu Gateway de acesso remoto e adicionar pré-autenticação para acesso remoto, você poderá revertê-la por meio do Proxy de aplicativo Web. Isso é uma excelente maneira para certificar-se de que ter o avançado pré-autenticação RDG incluindo MFA. Publicação sem pré-autenticação também é uma opção e fornece um único ponto de entrada em seus sistemas.  
  
#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>Como publicar um aplicativo em RDG usando a autenticação de passagem do Proxy de aplicativo Web  
  
1.  Instalação será diferente dependendo se o acesso via Web RD (/ rdweb) e as funções de Gateway de área de trabalho remota (rpc) estão no mesmo servidor ou em servidores diferentes.  
  
2.  Se as funções de acesso via Web RD e o Gateway de área de trabalho remota são hospedadas no mesmo servidor RDG, você pode simplesmente publicar a raiz do FQDN no Proxy de aplicativo Web, como https://rdg.contoso.com/.  
  
    Você também pode publicar os dois diretórios virtuais individualmente, por exemplo, https://rdg.contoso.com/rdweb/ e https://rdg.contoso.com/rpc/.  
  
3.  Se o acesso via Web de área de trabalho remota e o Gateway de área de trabalho remota são hospedadas em servidores separados, RDG, você precisa publicar os dois diretórios virtuais individualmente. Você pode usar o mesmo ou em outro FQDN externo, por exemplo, https://rdweb.contoso.com/rdweb/ e https://gateway.contoso.com/rpc/.  
  
4.  Se o FQDN interno e externo são diferentes, você não deve desabilitar conversão de cabeçalho de solicitação na regra de publicação RDWeb. Isso pode ser feito executando o seguinte script do PowerShell no servidor de Proxy de aplicativo Web, mas ele deve ser habilitado por padrão.
  
    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false  
    ```  
  
    > [!NOTE]  
    > Se você precisar dar suporte a clientes avançados, como o RemoteApp e conexões de área de trabalho ou iOS conexões de área de trabalho remota, eles não suportam pré-autenticação para que você precisa publicar RDG usando a autenticação de passagem.  
  
#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>Como publicar um aplicativo em RDG usando o Proxy de aplicativo Web com pré-autenticação  
  
1.  Web Proxy de aplicativo pré-autenticação com RDG funciona, passando o cookie de pré-autenticação obtido pelo Internet Explorer que está sendo passado para o cliente de Conexão de área de trabalho remota (mstsc.exe). Isso é usado pelo cliente de Conexão de área de trabalho remota (mstsc.exe). Isso é usado pelo cliente de Conexão de área de trabalho remota como prova de autenticação.  
  
    O procedimento a seguir informa o servidor de coleta para incluir propriedades personalizadas de RDP necessário nos arquivos de RDP de aplicativo remoto que serão enviados aos clientes. Estes informam o que pré-autenticação de cliente é necessário e passar os cookies para a pré-autenticação endereço do servidor para o cliente de Conexão de área de trabalho remota (mstsc.exe). Em conjunto com a desativação do recurso HttpOnly no aplicativo de Proxy de aplicativo Web, isso permite que o cliente de Conexão de área de trabalho remota (mstsc.exe) utilizar o cookie de Proxy de aplicativo Web obtido por meio do navegador.  
  
    Autenticação para o servidor de acesso via Web RD ainda será usar o logon de formulário do acesso via Web RD. Isso fornece o menor número de autenticação de usuário solicita conforme o formulário de logon do acesso via Web RD cria um repositório de credenciais de cliente que pode ser usado pelo cliente de Conexão de área de trabalho remota (mstsc.exe) para qualquer inicialização subsequente do aplicativo remoto.  
  
2.  Primeiro, crie um manual terceira parte confiável no AD FS como se você estivessem publicando um aplicativo com reconhecimento de declarações. Isso significa que você precisa criar uma cópia de confiança que está lá para impor a pré-autenticação, para que você obtenha pré-autenticação sem a delegação restrita de Kerberos para o servidor publicado da terceira parte confiável. Depois que um usuário autenticado, todo o resto é passado.  
  
    > [!WARNING]  
    > Pode parecer que usando a delegação é preferível, mas não resolve totalmente os requisitos de SSO mstsc e houver problemas ao delegar para o diretório /rpc porque espera que o cliente lidar com a autenticação de Gateway de área de trabalho remota em si.  
  
3.  Para criar um manual terceira parte confiável, siga as etapas no Console de gerenciamento do AD FS:  
  
    1.  Use o **adicionar terceira parte confiável** Assistente  
  
    2.  Selecione **inserir manualmente dados sobre a terceira**.  
  
    3.  Aceite todas as configurações padrão.  
  
    4.  Para o identificador de terceira parte confiável, digite o FQDN externo que você usará para acessar RDG, por exemplo https://rdg.contoso.com/.  
  
        Essa é a parte confiável que você usará ao publicar o aplicativo no Proxy de aplicativo Web.  
  
4.  Publicar a raiz do site (por exemplo, https://rdg.contoso.com/ ) no Proxy de aplicativo Web. Defina a pré-autenticação para o AD FS e use a terceira parte confiável criado anteriormente. Isso permitirá /rdweb e /rpc para usar o mesmo cookie de autenticação de Proxy de aplicativo Web.  
  
    É possível publicar /rdweb e /rpc como aplicativos separados e até mesmo usar diferentes servidores publicados. Basta garantir que você publique tanto usando a mesma terceira parte confiável como o token de Proxy de aplicativo Web é emitido para a terceira parte confiável e, portanto, é válido em todos os aplicativos publicados com o mesmo terceira parte confiável.  
  
5.  Se o FQDN interno e externo são diferentes, você não deve desabilitar conversão de cabeçalho de solicitação na regra de publicação RDWeb. Isso pode ser feito executando o seguinte script do PowerShell no servidor de Proxy de aplicativo Web, mas ele deve ser habilitado por padrão:
  
    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false 
    ```  
  
6.  Desabilitar a propriedade do cookie HttpOnly no Proxy de aplicativo da Web sobre o RDG aplicativo publicado. Para permitir o acesso de controle ActiveX RDG para o cookie de autenticação de Proxy de aplicativo Web, você precisará desabilitar a propriedade HttpOnly no cookie de Proxy de aplicativo Web.  
  
    Isso exige o seguinte a serem instalados [Hotfix de Proxy de aplicativo Web](https://support.microsoft.com/en-gb/kb/3000850) ou o [ https://support.microsoft.com/en-gb/kb/3000850 ](https://support.microsoft.com/en-gb/kb/3000850).  
  
    Depois de instalar o hotfix, execute o seguinte script do PowerShell no servidor de Proxy de aplicativo Web, especificando o nome do aplicativo relevante:  
  
    ```  
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true  
    ```  
  
    Desabilitar HttpOnly permite o acesso de controle de ActiveX RDG para o cookie de autenticação de Proxy de aplicativo Web.  
  
7.  Configure a coleta de RDG relevante no servidor de coleta para permitir que a Conexão de área de trabalho remota (mstsc.exe) do cliente sabe que a pré-autenticação é necessária no arquivo rdp.  
  
    -   No Windows Server 2012 e Windows Server 2012 R2, isso pode ser feito executando o seguinte cmdlet do PowerShell no servidor RDG coleção:  
  
        ```
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```
  
        Verifique se você remover o < e > colchetes ao substituir com seus próprios valores, por exemplo:
        
        ```
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```
  
    -   No Windows Server 2008 R2:  
  
        1.  Fazer logon no servidor de Terminal com uma conta que tenha privilégios de administrador.  
  
        2.  Vá para **inicie** >**ferramentas administrativas** > **serviços de Terminal** > **Gerenciador de TS RemoteApp.**  
  
        3.  No **visão geral** painel do Gerenciador de TS RemoteApp, ao lado de configurações de RDP, clique em **alteração**.  
  
        4.  Sobre o **configurações personalizadas de RDP** guia, digite as seguintes configurações de RDP na caixa de configurações personalizadas de RDP:  
  
            `pre-authentication server address: s: https://externalfqdn/rdweb/`  
  
            `require pre-authentication:i:1`  
  
        5.  Quando você terminar, clique em **aplicar**.  
  
            Isso informa o servidor de coleta para incluir as propriedades personalizadas de RDP nos arquivos de RDP que serão enviados aos clientes. Estes informam o que pré-autenticação de cliente é necessário e os cookies para a pré-autenticação de passar o endereço dos servidores para o cliente de Conexão de área de trabalho remota (mstsc.exe). Isso, em conjunto com desabilitando HttpOnly o aplicativo de Proxy de aplicativo Web, permite que o cliente de Conexão de área de trabalho remota (mstsc.exe) utilizar o cookie de autenticação de Proxy de aplicativo Web obtido por meio do navegador.  
  
            Para obter mais informações sobre o RDP, consulte [Configurando o cenário de OTP de Gateway TS](https://technet.microsoft.com/library/cc731249(v=ws.10).aspx).  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Planejando publicar aplicativos usando o Proxy de aplicativo Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))  
  
-   [Solução de problemas de Proxy de aplicativo Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))  
  
-   [Guia de instruções passo a passo de Proxy de aplicativo Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))  
