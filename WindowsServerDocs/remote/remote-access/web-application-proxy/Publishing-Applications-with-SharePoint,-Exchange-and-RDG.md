---
ms.assetid: 61ed00fd-51c7-4728-91fa-8501de9d8f28
title: Publicar aplicativos com SharePoint, Exchange e RDG
author: billmath
manager: mtillman
ms.author: billmath
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows-server
ms.technology: web-app-proxy
ms.openlocfilehash: 18851463b82afc1dc34615e6faaa14622c80224a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818679"
---
# <a name="publishing-applications-with-sharepoint-exchange-and-rdg"></a>Publicar aplicativos com SharePoint, Exchange e RDG

> Aplica-se a: Windows Server 2016

**Esse conteúdo é relevante para a versão local do proxy de aplicativo Web. Para habilitar o acesso seguro a aplicativos locais na nuvem, consulte o conteúdo de [proxy de aplicativo do AD do Azure](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**

Este tópico descreve as tarefas necessárias para publicar o SharePoint Server, o Exchange Server ou o gateway de Área de Trabalho Remota (RDP) por meio do proxy de aplicativo Web.

> [!NOTE]
> Essas informações são fornecidas no estado em que se encontram.  Serviços de Área de Trabalho Remota dá suporte e recomenda usar o [Proxy Azure app para fornecer acesso remoto seguro a aplicativos locais](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started).

## <a name="publish-sharepoint-server"></a><a name="BKMK_6.1"></a>Publicar o SharePoint Server
Você pode publicar um site do SharePoint por meio do proxy de aplicativo Web quando o site do SharePoint estiver configurado para autenticação baseada em declarações ou autenticação integrada do Windows. Se você quiser usar Serviços de Federação do Active Directory (AD FS) (AD FS) para pré-autenticação, deverá configurar uma terceira parte confiável usando um dos assistentes.

-   Se o site do SharePoint usa a autenticação baseada em declarações, você deve usar o Assistente para Adicionar o Objeto de Confiança de Terceira Parte Confiável para configurar o objeto de confiança de terceira parte confiável para o aplicativo.

-   Se o site do SharePoint usa a autenticação integrada do Windows, você deve usar o Assistente para Adicionar o Objeto de Confiança de Terceira Parte Confiável Não Baseado em Declarações para configurar o objeto de confiança de terceira parte confiável para o aplicativo. Você pode usar a IWA com um aplicativo Web baseado em declarações desde que configure o KDC.

    Para permitir que os usuários se autentiquem usando a autenticação integrada do Windows, o servidor proxy de aplicativo Web deve ser Unido a um domínio.

    Você deve configurar o aplicativo para dar suporte à delegação restrita de Kerberos. Você pode fazer isso no controlador de domínio de qualquer aplicativo. Você também pode configurar o aplicativo diretamente no servidor de back-end se ele estiver em execução no Windows Server 2012 R2 ou no Windows Server 2012. Para obter mais informações, consulte [Novidades na autenticação Kerberos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11)). Você também deve certificar-se de que os servidores proxy de aplicativo Web estejam configurados para delegação para os nomes de entidade de serviço dos servidores de back-end. Para obter uma explicação de como configurar o proxy de aplicativo Web para publicar um aplicativo usando a autenticação integrada do Windows, consulte [configurar um site para usar a autenticação integrada do Windows](https://docs.microsoft.com/previous-versions/orphan-topics/ws.11/dn308246(v=ws.11)).

Se seu site do SharePoint for configurado usando AAM (mapeamentos alternativos de acesso) ou coleções de sites com nome de host, você pode usar diferentes URLs de servidor externo e de back-end para publicar seu aplicativo. No entanto, se você não configurar seu site do SharePoint usando o AAM ou coleções de sites com nome de host, deve usar as mesmas URLs de servidor externo e de back-end.

## <a name="publish-exchange-server"></a><a name="BKMK_6.2"></a>Publicar o Exchange Server
A tabela a seguir descreve os serviços do Exchange que você pode publicar por meio do proxy de aplicativo Web e a pré-autenticação com suporte para esses serviços:


|    Serviço do Exchange    |                                                                            Pré-autenticação                                                                            |                                                                                                                                       {1&gt;Observações&lt;1}                                                                                                                                        |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Outlook Web App     | -AD FS usando a autenticação não baseada em declarações<br />-Passagem<br />-AD FS usando a autenticação baseada em declarações para o Exchange 2013 Service Pack 1 (SP1) local |                                                                  Para obter mais informações, consulte [Usando autenticação baseada em declarações do AD FS com o Outlook Web App e o EAC](https://go.microsoft.com/fwlink/?LinkId=393723)                                                                  |
| Painel de controle do Exchange |                                                                               Passagem                                                                               |                                                                                                                                                                                                                                                                                    |
|    Outlook em Qualquer Lugar    |                                                                               Passagem                                                                               | Você deve publicar as três URLs para o Outlook em Qualquer Lugar funcionar corretamente:<p>-A URL de descoberta automática.<br />-O nome de host externo do servidor Exchange; ou seja, a URL que está configurada para os clientes se conectarem.<br />-O FQDN interno do Exchange Server. |
|  Exchange ActiveSync   |                                                     Passagem<br/> AD FS usando o protocolo HTTP Basic Authorization                                                      |                                                                                                                                                                                                                                                                                    |

Para publicar o Outlook Web App usando a autenticação integrada do Windows, você deve usar o Assistente para Adicionar o Objeto de Confiança de Terceira Parte Confiável Não Baseado em Declarações para configurar o objeto de confiança de terceira parte confiável para o aplicativo.

Para permitir que os usuários se autentiquem usando a delegação restrita de Kerberos, o servidor proxy de aplicativo Web deve ser Unido a um domínio.

Você deve configurar o aplicativo para dar suporte à autenticação Kerberos. Além disso, você precisa registrar um SPN (nome da entidade de serviço) na conta em que o serviço Web está sendo executado. Você pode fazer isso no controlador de domínio ou nos servidores de back-end. Em um ambiente do Exchange com balanceamento de carga, isso exigiria usar a conta de serviço alternativa, consulte [Configurando a autenticação Kerberos para servidores de acesso para cliente com balanceamento de carga](https://docs.microsoft.com/exchange/configuring-kerberos-authentication-for-load-balanced-client-access-servers-exchange-2013-help)

Você também pode configurar o aplicativo diretamente no servidor de back-end se ele estiver em execução no Windows Server 2012 R2 ou no Windows Server 2012. Para obter mais informações, consulte [Novidades na autenticação Kerberos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831747(v=ws.11)). Você também deve certificar-se de que os servidores proxy de aplicativo Web estejam configurados para delegação para os nomes de entidade de serviço dos servidores de back-end.

## <a name="publishing-remote-desktop-gateway-through-web-application-proxy"></a>Publicando o gateway de Área de Trabalho Remota por meio do proxy de aplicativo Web
Se você quiser restringir o acesso ao gateway de acesso remoto e adicionar pré-autenticação para acesso remoto, poderá distribuí-lo por meio do proxy de aplicativo Web. Essa é uma maneira realmente boa de garantir que você tenha a pré-autenticação avançada para RDG, incluindo MFA. Publicar sem pré-autenticação também é uma opção e fornece um ponto único de entrada em seus sistemas.

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-pass-through-authentication"></a>Como publicar um aplicativo no RDG usando a autenticação de passagem do proxy de aplicativo Web

1. A instalação será diferente dependendo se suas funções de Acesso via Web de trabalho remota (/RDWeb) e de gateway de área de trabalho remota (RPC) estão no mesmo servidor ou em servidores diferentes.

2. Se as funções RD Acesso via Web e RD Gateway estiverem hospedadas no mesmo servidor RDG, você poderá simplesmente publicar o FQDN raiz no proxy de aplicativo Web, como https://rdg.contoso.com/.

   Você também pode publicar os dois diretórios virtuais individualmente, por exemplo,<https://rdg.contoso.com/rdweb/> e https://rdg.contoso.com/rpc/.

3. Se o Acesso via Web RD e o gateway de área de trabalho remota estiverem hospedados em servidores RDG separados, você precisará publicar os dois diretórios virtuais individualmente. Você pode usar o mesmo ou FQDN externo diferente, por exemplo, https://rdweb.contoso.com/rdweb/ e https://gateway.contoso.com/rpc/.

4. Se os FQDN externos e internos forem diferentes, você não deverá desabilitar a tradução do cabeçalho de solicitação na regra de publicação RDWeb. Isso pode ser feito executando o seguinte script do PowerShell no servidor proxy de aplicativo Web, mas ele deve ser habilitado por padrão.

   ```PowerShell
   Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false
   ```

   > [!NOTE]
   > Se você precisar dar suporte a clientes avançados, como conexões RemoteApp e de área de trabalho ou conexões de Área de Trabalho Remota do iOS, eles não dão suporte à pré-autenticação para que você tenha que publicar RDG usando a autenticação de passagem.

#### <a name="how-to-publish-an-application-in-rdg-using-web-application-proxy-with-pre-authentication"></a>Como publicar um aplicativo no RDG usando o proxy de aplicativo Web com pré-autenticação

1.  A pré-autenticação do proxy de aplicativo Web com o RDG funciona passando o cookie de pré-autenticação obtido pelo Internet Explorer que está sendo passado para o cliente de Conexão de Área de Trabalho Remota (mstsc. exe). Isso é usado pelo cliente do Conexão de Área de Trabalho Remota (mstsc. exe). Isso é usado pelo cliente do Conexão de Área de Trabalho Remota como prova de autenticação.

    O procedimento a seguir informa o servidor de coleta para incluir as propriedades de RDP personalizadas necessárias nos arquivos RDP do aplicativo remoto que são enviadas aos clientes. Eles dizem ao cliente que a pré-autenticação é necessária e passa os cookies para o endereço do servidor de pré-autenticação para Conexão de Área de Trabalho Remota cliente (mstsc. exe). Em conjunto com a desabilitação do recurso HttpOnly no aplicativo de proxy de aplicativo Web, isso permite que o cliente Conexão de Área de Trabalho Remota (mstsc. exe) Utilize o cookie de proxy de aplicativo Web obtido por meio do navegador.

    A autenticação para o servidor de Acesso via Web de área de trabalho remota ainda usará o logon do formulário RD Acesso via Web. Isso fornece o menor número de prompts de autenticação de usuário, pois o formulário de logon de Acesso via Web RD cria um armazenamento de credenciais do lado do cliente que pode ser usado pelo cliente do Conexão de Área de Trabalho Remota (mstsc. exe) para qualquer inicialização de aplicativo remoto subsequente.

2.  Primeiro, crie uma terceira parte confiável manual em AD FS como se estivesse publicando um aplicativo com reconhecimento de declarações. Isso significa que você precisa criar uma terceira parte confiável fictícia que esteja lá para impor pré-autenticação, para que você obtenha pré-autenticação sem a delegação restrita de Kerberos para o servidor publicado. Depois que um usuário for autenticado, todo o restante será passado.

    > [!WARNING]
    > Pode parecer que usar a delegação é preferível, mas não resolve totalmente os requisitos de SSO do mstsc e há problemas ao delegar para o diretório/RPC porque o cliente espera tratar a própria autenticação do gateway RD.

3.  Para criar uma relação de confiança de terceira parte confiável manual, siga as etapas no console de gerenciamento do AD FS:

    1.  Usar o assistente para **Adicionar confiança de terceira parte confiável**

    2.  Selecione **inserir dados sobre a terceira parte confiável manualmente**.

    3.  Aceite todas as configurações padrão.

    4.  Para o identificador de confiança de terceira parte confiável, insira o FQDN externo que você usará para acesso RDG, por exemplo https://rdg.contoso.com/.

        Essa é a terceira parte confiável que será usada ao publicar o aplicativo no proxy de aplicativo Web.

4.  Publique a raiz do site (por exemplo, https://rdg.contoso.com/) no proxy de aplicativo Web. Defina a pré-autenticação como AD FS e use a confiança de terceira parte confiável que você criou acima. Isso permitirá/RDWeb e/RPC usarem o mesmo cookie de autenticação de proxy de aplicativo Web.

    É possível publicar/RDWeb e/RPC como aplicativos separados e até mesmo usar diferentes servidores publicados. Você só precisa garantir que você publique usando a mesma relação de confiança de terceira parte confiável que o token do proxy de aplicativo Web é emitido para a relação de confiança de terceira parte confiável e, portanto, válido entre aplicativos publicados com a mesma confiança de terceira parte confiável.

5.  Se os FQDN externos e internos forem diferentes, você não deverá desabilitar a tradução do cabeçalho de solicitação na regra de publicação RDWeb. Isso pode ser feito executando o seguinte script do PowerShell no servidor proxy de aplicativo Web, mas deve ser habilitado por padrão:

    ```PowerShell
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableTranslateUrlInRequestHeaders:$false
    ```

6.  Desabilite a propriedade de cookie HttpOnly no proxy de aplicativo Web no aplicativo publicado RDG. Para permitir que o controle ActiveX RDG o acesso ao cookie de autenticação de proxy de aplicativo Web, você precisa desabilitar a propriedade HttpOnly no cookie de proxy de aplicativo Web.

    Isso exige que você instale o [pacote cumulativo de atualizações de novembro de 2014 para Windows RT 8,1, Windows 8.1 e Windows Server 2012 R2 (KB3000850)](https://support.microsoft.com/kb/3000850).

    Depois de instalar o hotfix, execute o seguinte script do PowerShell no servidor proxy de aplicativo Web especificando o nome do aplicativo relevante:

    ```PowerShell
    Get-WebApplicationProxyApplication applicationname | Set-WebApplicationProxyApplication -DisableHttpOnlyCookieProtection:$true
    ```

    Desabilitar o HttpOnly permite que o controle ActiveX RDG o acesso ao cookie de autenticação de proxy de aplicativo Web.

7.  Configure a coleção RDG relevante no servidor de coleta para permitir que o cliente do Conexão de Área de Trabalho Remota (mstsc. exe) saiba que a pré-autenticação é necessária no arquivo RDP.

    -   No Windows Server 2012 e no Windows Server 2012 R2, isso pode ser feito executando o seguinte cmdlet do PowerShell no servidor de coleta RDG:

        ```PowerShell
        Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s: <https://externalfqdn/rdweb/>`nrequire pre-authentication:i:1"
        ```

        Certifique-se de remover os < e > colchetes quando substituir pelos seus próprios valores, por exemplo:

        ```PowerShell
        Set-RDSessionCollectionConfiguration -CollectionName "MyAppCollection" -CustomRdpProperty "pre-authentication server address:s: https://rdg.contoso.com/rdweb/`nrequire pre-authentication:i:1"
        ```

    -   No Windows Server 2008 R2:

        1.  Faça logon na Terminal Server com uma conta que tenha privilégios de administrador.

        2.  Acesse **iniciar** >**Ferramentas administrativas** > **serviços de terminal** > **Gerenciador de TS RemoteApp.**

        3.  No painel **visão geral** do gerenciador de TS RemoteApp, ao lado de configurações de RDP, clique em **alterar**.

        4.  Na guia **configurações personalizadas de RDP** , digite as seguintes configurações de RDP na caixa configurações personalizadas de RDP:

            `pre-authentication server address: s: https://externalfqdn/rdweb/`

            `require pre-authentication:i:1`

        5.  Quando terminar, clique em **aplicar**.

            Isso informa ao servidor de coleta para incluir as propriedades personalizadas de RDP nos arquivos RDP que são enviadas aos clientes. Eles informam ao cliente que a pré-autenticação é necessária e passa os cookies para o endereço do servidor de pré-autenticação para o cliente de Conexão de Área de Trabalho Remota (mstsc. exe). Isso, em conjunto com a desabilitação do HttpOnly no aplicativo de proxy de aplicativo Web, permite que o cliente de Conexão de Área de Trabalho Remota (mstsc. exe) Utilize o cookie de autenticação de proxy de aplicativo Web obtido por meio do navegador.

            Para obter mais informações sobre o RDP, consulte [Configurando o cenário de OTP do Gateway TS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731249(v=ws.10)).

## <a name="see-also"></a><a name="BKMK_Links"></a>Consulte também

- [Planejando a publicação de aplicativos usando o proxy de aplicativo Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383650(v=ws.11))

- [Como solucionar problemas de proxy de aplicativo Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn770156(v=ws.11))

- [Guia explicativo do proxy de aplicativo Web](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))
