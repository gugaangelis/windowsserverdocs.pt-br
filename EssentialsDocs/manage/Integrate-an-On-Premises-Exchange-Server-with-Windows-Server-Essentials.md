---
title: Integrar um servidor do Exchange no local com o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56a21e2-c9e3-4ba9-97d9-719ea6a0854b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c2020e08b94800a9750f095a2f772afb14ba5f0b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>Integrar um servidor do Exchange no local com o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este guia fornece informações e instruções básicas para ajudá-lo a configurar e integrar um servidor local que esteja executando o Exchange Server com um servidor que está executando o Windows Server Essentials.  
  
 Você deve ler este guia antes de tentar implantar um servidor local que esteja executando o Exchange Server em uma rede do Windows Server Essentials.  
  
> [!NOTE]
>  Exchange Server 2010 não dá suporte a instalação em computadores que executam o Windows Server 2012.  
  
## <a name="prerequisites"></a>Pré-requisitos  
 Antes de instalar o servidor do Exchange em uma rede do Windows Server Essentials, certifique-se de que você concluir as tarefas descritas nesta seção.  
  
-   [Configurar um servidor que está executando o Windows Server Essentials](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)  
  
-   [Preparar um segundo servidor no qual deseja instalar o servidor do Exchange](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)  
  
-   [Configurar o nome de domínio da Internet](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)  
  
###  <a name="BKMK_SetUpSBS8"></a>Configurar um servidor que está executando o Windows Server Essentials  
 Você deve já configurou um servidor que está executando o Windows Server Essentials. Esse será o controlador de domínio para o servidor que está executando o Exchange Server. Para obter informações sobre como configurar o Windows Server Essentials, consulte [instalar o Windows Server Essentials](../install/Install-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_SecondServer"></a>Preparar um segundo servidor no qual deseja instalar o servidor do Exchange  
 Você deve instalar o servidor do Exchange em um segundo servidor que está executando uma versão do sistema operacional Windows Server que oficialmente dá suporte à execução do Exchange Server 2010 ou o Exchange Server 2013. Em seguida, você deve associar o segundo servidor no domínio do Windows Server Essentials.  
  
 Para obter informações sobre como ingressar em um segundo servidor no domínio do Windows Server Essentials, consulte ingressar em um segundo servidor de rede em [conectado](../use/Get-Connected-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Microsoft não oferece suporte à instalação do servidor do Exchange em um servidor que está executando o Windows Server Essentials.  
  
###  <a name="BKMK_DomainNames"></a>Configurar o nome de domínio da Internet  
 Para integrar um servidor local que esteja executando o Exchange Server com o Windows Server Essentials, você deve ter registrado um nome de domínio Internet válido para sua empresa (como *contoso.com*). Você também deve trabalhar com seu provedor de nomes de domínio para criar os registros de recurso DNS Exchange Server requer.  
  
 Por exemplo, se o nome de domínio da Internet sua empresa é contoso.com e quiser usar o nome de domínio totalmente qualificado (FQDN) do *mail.contoso.com* para fazer referência a seu servidor de local que está executando o Exchange Server, trabalhar com seu provedor de nomes de domínio para criar os registros de recurso DNS na tabela a seguir.  
  
|Nome de registro de recurso|Tipo de registro|Configuração do registro|Descrição|  
|--------------------------|-----------------|--------------------|-----------------|  
|mail|host (A)|Endereço =*endereço IP público atribuído por seu ISP*|Exchange Server receberá emails endereçados para mail.contoso.com.<br /><br /> Você pode usar um nome diferente em seu próprio seleção.|  
|MX|servidor de mensagens (MX)|Nome do host = @<br /><br /> Address=mail.contoso.com<br /><br /> Preferência = 0|Fornece o roteamento de mensagens de email para email@contoso.com para chegar ao seu servidor de local que está executando o Exchange Server.|  
|SPF|texto (TXT)|v = spf1 um mx ~ todos|Registro de recurso que ajuda a impedir emails enviados de seu servidor como sendo identificadas como spam.|  
|Autodiscover._tcp|serviços (SRV)|Serviço: _autodiscover<br /><br /> Protocolo: TCP<br /><br /> Prioridade: 0<br /><br /> Peso: 0<br /><br /> Porta: 443<br /><br /> Host de destino: mail.contoso.com|Permite que o Microsoft Office Outlook e dispositivos móveis detectar automaticamente o servidor local que esteja executando o Exchange Server.<br /><br /> **Observação:** você também pode configurar um registro de recurso de host (A) descoberta automática e aponte o registro para o endereço IP público do seu servidor local que esteja executando o Exchange Server. No entanto, se você implementar essa opção, você deve também fornecer certificado SSL do assunto alternativos nome (SAN) que dá suporte a mail.contoso.com e autodiscover.contoso.com nomes de domínio.|  
  
> [!NOTE]
>  -   Substitua as instâncias de *contoso.com* neste exemplo com o nome de domínio da Internet que você registrou.  
  
 Você deve escolher um FQDN diferente do servidor local que esteja executando o servidor do Exchange que o FQDN que você está usando para o servidor que está executando o Windows Server Essentials. Por exemplo, você pode optar por usar *remote.contoso.com* como o FQDN computadores usam para acessar o servidor que executa o Windows Server Essentials da Internet. Você pode usar *mail.contoso.com* como o FQDN que é usado para rotear email ao seu servidor local que esteja executando o Exchange Server.  
  
## <a name="install-exchange-server"></a>Instalar o Exchange Server  
 O recurso de integração do Exchange Server no Windows Server Essentials compatível com as seguintes versões do Exchange Server:  
  
-   Exchange Server 2013  
  
-   Exchange Server 2010 com Service Pack 1 (SP1)  
  
 Antes de instalar o servidor do Exchange no segundo servidor, primeiro você deve adicionar a conta de administrador atual para o **administradores corporativos** grupo.  
  
#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>Para adicionar a conta de administrador atual ao grupo Administradores corporativos  
  
1.  Faça logon no Windows Server Essentials como administrador.  
  
2.  Execute o Windows PowerShell como administrador.  
  
3.  No prompt de comando do Windows PowerShell, digite **˜Enterprise ADGroupMember adicionar administradores $env: nome de usuário**, e pressione Enter.  
  
#### <a name="to-install-exchange-server"></a>Para instalar o servidor do Exchange  
  
1.  Faça logon para o segundo servidor como administrador.  
  
2.  Abra seu navegador da Internet e, em seguida, navegue até o [Assistente de implantação do Exchange Server](https://go.microsoft.com/fwlink/p/?LinkID=249163) site.  
  
3.  Clique em **local apenas**.  
  
4.  Clique na opção de instalação de novo para a versão do servidor do Exchange que você vai instalar.  
  
    > [!NOTE]
    >  Se você estiver migrando em uma instalação do Windows Small Business Server, você deve selecionar a opção de atualização adequada que abrange as etapas de migração.  
  
5.  Na próxima página, aceite as configurações padrão e, em seguida, clique em **próxima**.  
  
    > [!NOTE]
    >  Se você pretende usar pastas públicas na nova instalação do Exchange Server, alterar essa configuração para **Sim**.  
  
6.  Siga as instruções passo a passo da lista de verificação para implantar o Exchange Server.  
  
     O Assistente de implantação do servidor do Exchange também permite que você:  
  
    -   Imprima uma cópia da lista de verificação.  
  
    -   Envie uma cópia da lista de verificação para um destinatário do email.  
  
    -   Baixe a lista de verificação como um arquivo PDF.  
  
> [!NOTE]
>  -   Você deve optar por instalar as ferramentas de gerenciamento no servidor que está executando o Exchange Server. As ferramentas de gerenciamento são exigidas pelo recurso de integração com o Exchange Server no Windows Server Essentials.  
> -   Se você precisa configurar diretórios virtuais, recomendamos que você defina também o **InternalUrl** propriedade para ser a mesma URL como o **ExternalUrl** propriedade para cada diretório virtual. Para obter mais informações, consulte [gerenciamento de cliente acesso Virtual diretórios de servidor](https://go.microsoft.com/fwlink/p/?LinkId=251058) no site de Ajuda online do Exchange Server 2010.  
> -   Se você quiser acessar o Outlook Web Access (OWA) de dentro do site de acesso via Web remoto no Windows Server Essentials, você deve definir a propriedade URL externa para OWA.  
  
 Se você estiver instalando o Exchange Server 2010 em uma instalação limpa, você também pode usar os scripts a seguir para configurar o servidor do Exchange.  
  
#### <a name="to-use-scripts-to-set-up-exchange-server"></a>Usar scripts para configurar o servidor do Exchange  
  
1.  Abra o bloco de notas e cole este script em um novo arquivo:  
  
     `Import-Module ServerManager`  
  
     `Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  �Restart`  
  
2.  Salve o arquivo como **InstallDependencies.ps1**.  
  
3.  Copie o certificado SSL do Exchange para um local no servidor.  
  
4.  Abra um novo arquivo de bloco de notas e copie o seguinte texto para o arquivo:  
  
     `param (`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]`  
  
     `$CertPath = "c:\certificates\ExchangeCertificate.pfx",`  
  
     `[Security.SecureString]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]`  
  
     `$CertPassword = $null,`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]`  
  
     `$DomainName = "contoso.com",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]`  
  
     `$ServerIpAddress = "192.168.0.1",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]`  
  
     `$InternalIpRange = "192.168.0.0-192.168.0.255"`  
  
     `)`  
  
     `#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.`  
  
     `Import-ExchangeCertificate  �FileData ([Byte[]]$(Get-content -Path $CertPath  �Encoding byte  �ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force`  
  
     `#New AcceptedDomain and set it to default`  
  
     `New-AcceptedDomain  �Name "official name"  �DomainName $domainname`  
  
     `Set-AcceptedDomain  �Identity "official name"  �MakeDefault $true`  
  
     `#New EmailAddress Policy`  
  
     `$address = "%m@" + $DomainName`  
  
     `New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address`  
  
     `#Set owa and ecp VirtualDirectory ExternalUrl`  
  
     `$hostname = "mail." + $DomainName`  
  
     `$owa = "https://" + $hostname + "/owa"`  
  
     `$ecp = "https://" + $hostname + "/ecp"`  
  
     `$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"`  
  
     `$oab = "https://" + $hostname + "/OAB"`  
  
     `$ews = "https://" + $hostname + "/EWS/Exchange.asmx"`  
  
     `Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  �ExternalUrl $owa  �InternalUrl $owa`  
  
     `Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  �ExternalUrl $ecp  �InternalUrl $ecp`  
  
     `Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  �InternalUrl $activesync`  
  
     `Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true`  
  
     `Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force`  
  
     `#Enable outlook Anywhere`  
  
     `Enable-OutlookAnywhere  �ClientAuthenticationMethod:Basic  �ExternalHostname:$hostname  �SSLOffloading:$false`  
  
     `#new receive/send connector`  
  
     `$machinename = get-content env:computername`  
  
     `$bindingIpaddress = $ServerIpAddress + ":25"`  
  
     `$RecevieConnectorName = $machinename + "\Default " + $machinename`  
  
     `Set-ReceiveConnector $RecevieConnectorName -RemoteIPRanges $InternalIpRange`  
  
     `New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated`  
  
     `New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename`  
  
5.  Defina os parâmetros no início do script para refletir seu ambiente de rede.  
  
6.  Salve o arquivo como **ConfigureExchange.ps1**.  
  
7.  Execute o Windows PowerShell como administrador.  
  
8.  No prompt de comando do Windows PowerShell, digite **Set-ExecutionPolicy RemoteSigned**, e pressione Enter.  
  
9. Execute o script **InstallDependencies.ps1**.  
  
10. Reinicie o servidor e executar o Windows PowerShell como administrador.  
  
11. No prompt de comando do Windows PowerShell, execute o script a seguir:  
  
     `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`  
  
    > [!NOTE]
    >  Certifique-se de digitar o caminho correto para o programa de instalação do servidor do Exchange.  
  
12. Quando a instalação do Exchange Server for concluída, abra o Shell de gerenciamento do Exchange como um administrador.  
  
13. No prompt de comando de Shell de gerenciamento do Exchange, digite **Set-ExecutionPolicy RemoteSigned**, e pressione Enter.  
  
14. Execute o script **ConfigureExchange.ps1**.  
  
15. Reinicie o servidor.  
  
> [!NOTE]
>  Se você decidir usar um certificado SSL publicamente confiável em vez de um certificado emitido por conta própria, você pode seguir as instruções no guia de instalação para criar uma solicitação de certificado e enviá-lo para sua autoridade de certificação selecionado. Você também pode usar um cmdlet do PowerShell do Exchange para criar uma solicitação de certificado. Segue um exemplo.  
>   
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`  
>   
>  Personalize os parâmetros de script para refletir seu ambiente de rede.  
  
## <a name="post-installation-tasks"></a>Tarefas pós-instalação  
 Esta seção descreve as tarefas de configuração de servidor, que talvez seja necessário para concluir na fase de pós-instalação que contém informações específicas ao configurar um servidor local que esteja executando o Exchange Server em uma rede do Windows Server Essentials.  
  
### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>Adicione o domínio de email pública e configurar as políticas de endereço de email  
  
> [!NOTE]
>  Essa é uma tarefa necessária se você estiver executando uma instalação limpa. Se você estiver migrando do Windows Small Business Server, ignore esta etapa.  
  
 Você deve especificar seu domínio de email para ser o domínio aceito padrão e, em seguida, configurar a política de endereço de email.  
  
##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>Para adicionar seu domínio de email como o padrão do domínio aceito  
  
1.  Siga as instruções no artigo do Exchange Server [criar um domínio aceito](https://go.microsoft.com/fwlink/p/?LinkId=249174) para adicionar um domínio aceito.  
  
2.  Logon para o segundo servidor como administrador, abra o Console de gerenciamento do Exchange e, em seguida, navegue até o **Hub de transporte** guia a **configuração da organização**.  
  
3.  No painel de trabalho do Console de gerenciamento do Exchange, clique com botão direito do novo domínio aceito e clique em **definir como padrão**.  
  
4.  Siga as instruções no artigo do Exchange Server [criar uma política de endereço de Email](https://go.microsoft.com/fwlink/p/?LinkId=249179) para criar uma nova política de endereço de email. Você pode aceitar todos os valores padrão, exceto o endereço de email. Para o endereço de email, especifique seu domínio de email pública.  
  
### <a name="create-smtp-send-and-receive-connectors"></a>Criar SMTP enviar e receber conectores  
  
> [!NOTE]
>  Essa é uma tarefa necessária.  
  
 Você deve configurar um conector de envio SMTP e um conector de recebimento SMTP para transmissão de entrada/saída de mensagens de email.  
  
 Para criar um conector de envio SMTP, siga as instruções no artigo do Exchange Server [criar um conector de envio SMTP](https://technet.microsoft.com/library/aa997285.aspx).  
  
 Para criar um conector de recebimento SMTP, siga as instruções no artigo do Exchange Server [criar um conector de receber SMTP](https://technet.microsoft.com/library/bb125159.aspx).  
  
 Como uma opção, você pode se referir ao script anteriormente contidas neste documento para criar a enviar e receber conectores usando os cmdlets do PowerShell do Exchange.  
  
### <a name="configure-the-network-router"></a>Configurar o roteador de rede  
  
> [!NOTE]
>  Essa é uma tarefa necessária se você estiver executando uma instalação limpa. Se você estiver migrando do Windows Small Business Server, consulte [migrar dados do servidor para Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md) para obter instruções sobre como configurar a rede.  
  
 No mínimo, você deve configurar as seguintes configurações de porta no roteador:  
  
|Porta do roteador|Destino IP|Porta de destino|Observação|  
|-----------------|--------------------|----------------------|----------|  
|25 (SMTP)|IP interno do servidor local que esteja executando o Exchange Server.|25||  
|80 (HTTP)|IP interno do servidor que está executando o Windows Server Essentials|80||  
|443 (HTTPS)|IP interno do servidor que está executando o Windows Server Essentials|443||  
  
 Se você der suporte a POP3 ou IMAP mensagens protocolos em sua rede, você também deve configurar forwardings de porta para esses protocolos. Para obter informações relacionadas, consulte a seção **servidores de acesso do cliente** no tópico [referência de porta de rede do Exchange](https://go.microsoft.com/fwlink/p/?LinkId=250773) na biblioteca do TechNet do Exchange Server.  
  
> [!NOTE]
>  -   Recomendamos que você configure endereços IP estáticos para o servidor que está executando o Windows Server Essentials e para o segundo servidor que está executando o Exchange Server. Para obter instruções sobre como configurar um endereço IP estático em um computador executando o Windows Server 2003 ou Windows Server 2008 R2, consulte [configurar um endereço IP estático](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx) na biblioteca do TechNet do Windows Server  
>   
>      **Observação:** a configuração do servidor DNS sempre deve apontar para o endereço IP do servidor que está executando o Windows Server Essentials.  
> -   No roteador, certifique-se de que os endereços IP do servidor que está executando o Windows Server Essentials e o servidor que está executando o Exchange Server são reservados, ou que estão fora do intervalo de endereços IP DHCP.  
> -   A configuração do roteador nesta seção pressupõe que você tenha apenas um endereço IP público atribuído pelo provedor de serviços de Internet (ISP). Se você tiver vários endereços IP públicos, você pode atribuir um endereço IP diferente para o servidor que está executando o Windows Server Essentials e ao servidor que está executando o Exchange Server e, em seguida, criar forwardings porta com base nos endereços IP públicos.  
  
### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>Habilitam a integração do servidor do Exchange no local no Windows Server Essentials  
  
> [!NOTE]
>  Se você estiver migrando de uma instalação do Windows Small Business Server, recomendamos que você ignore esta etapa por enquanto e executá-lo após a desinstalação da instalação anterior do Exchange Server no servidor de origem.  
  
 Depois de instalar e configurar um servidor que está executando o Exchange Server, você deve habilitar a integração do servidor do Exchange no local no servidor que está executando o Windows Server Essentials.  
  
##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>Para habilitar a integração do servidor do Exchange no local do painel  
  
1.  Fazer logon no servidor que está executando o Windows Server Essentials como administrador e, em seguida, abra o painel.  
  
2.  Sobre o **Home** página, clique em **conectar ao meu serviço de Email**e, em seguida, clique em **integrar o Exchange Server**.  
  
3.  No painel de informações, clique em **configurar a integração com o Exchange Server**.  
  
4.  Siga as instruções no assistente.  
  
### <a name="configure-a-reverse-proxy"></a>Configurar um proxy inverso  
  
> [!NOTE]
>  Essa é uma tarefa necessária se você tiver apenas uma conexão de Internet do seu provedor de serviços de Internet.  
  
 Windows Server Essentials e Exchange Server oferecer suporte a alguns cenários de acesso remoto para usuários de rede. Por exemplo, se você ativar acesso em qualquer local no servidor que está executando o Windows Server Essentials, você pode acessar o site de acesso via Web remoto ou remotamente usar rede virtual privada (VPN) para se conectar remotamente com a rede do Windows Server Essentials. Para acessar remotamente mensagens de email, você deve usar o Outlook em qualquer lugar, Outlook Web Access (OWA) ou o ActiveSync.  
  
 Se o Windows Server Essentials e o servidor que executa o Exchange Server estão ambos conectados ao mesmo roteador e não há somente uma entrada Internet conexão do seu provedor de serviços de Internet ao roteador, você deve usar uma solução de proxy inverso rotear diferentes tipos de solicitações de acesso remoto da Internet com base nos nomes de host de destino. Recomendamos que você use o Microsoft suporte extensão IIS aplicativo solicitar roteamento (ARR) como sua solução de proxy inverso. Para obter mais informações sobre o roteamento de solicitação de aplicativo do IIS, visite o [aplicativo solicitar roteamento site](https://go.microsoft.com/fwlink/p/?LinkId=249181).  
  
##### <a name="to-install-and-configure-application-request-routing"></a>Para instalar e configurar o aplicativo solicitar roteamento  
  
1.  Faça logon no Windows Server Essentials como administrador.  
  
2.  Abra seu navegador da Internet e navegue até o [aplicativo solicitar roteamento site](https://go.microsoft.com/fwlink/p/?LinkId=249181).  
  
3.  No site do ARR, clique em **instalar**e siga as instruções para instalar seta com  
  
    > [!NOTE]
    >  Você deve selecionar o URL reescreva módulo durante a instalação ARR.  
    >   
    >  Você pode receber um erro ao final da instalação do ARR 2589179 KB para ARR 2.5 não foi instalado com êxito. Você pode ignorar com segurança esse erro.  
  
4.  Quando ARR instalação for concluída, reinicie o **Gateway de área de trabalho remota** serviço se ele não estiver em execução.  
  
    > [!NOTE]
    >  Depois de instalar o ARR, o **Gateway de área de trabalho remota** serviço pode ser interrompido. Para reiniciar manualmente o serviço, abra o **serviços** ferramenta administrativa e reinicie o **Gateway de área de trabalho remota** serviço.  
  
5.  [Baixe KB2732764 para ARR 2.5](https://go.microsoft.com/fwlink/?LinkID=258302)e então instalar a atualização no servidor que está executando o Windows Server Essentials.  
  
6.  Copie o arquivo de certificado SSL para Exchange Server para o servidor que está executando o Windows Server Essentials. O arquivo do certificado deve conter a chave privada e ele deve ser no formato de arquivo PFX.  
  
    > [!NOTE]
    >  Se você estiver usando um certificado emitido por conta própria, siga as instruções no artigo do Exchange Server [exportar um certificado de troca](https://technet.microsoft.com/library/dd351274.aspx) para exportar o certificado.  
  
7.  Dependendo de qual versão do Windows Server Essentials que você está executando, siga um destes procedimentos:  
  
    -   No Windows Server Essentials: Abra uma janela de comando como administrador e, em seguida, abra a pasta %ProgramFiles%\Windows Server\Bin  
  
    -   No Windows Server Essentials: Abra uma janela de comando como administrador e, em seguida, abra o diretório de %Windir%\System32\Essentials.  
  
8.  Com base no seu cenário de instalação, siga uma destas etapas para configurar ARR:  
  
    -   Se você estiver executando uma instalação limpa, execute o seguinte comando:  
  
         * * ARRConfig config cert * * *caminho do arquivo de certificado* * * hostnames * * *hospedar nomes para Exchange Server* ****  
  
        > [!NOTE]
        >  Por exemplo; * * ARRConfig config cert ***c:\temp\certificate.pfx*** ***mail.contoso.com*** hostnames  
        >   
        >  Substitua *mail.contoso.com* com o nome do seu domínio protegido pelo certificado.  
  
    -   Em que você está migrando do Windows Small Business Server, execute o seguinte comando:  
  
         * * ARRConfig config cert * * *caminho do arquivo de certificado* * * hostnames * * *hospedar nomes para Exchange Server* * * servidor de destino * * *nome do servidor do Exchange Server* ****  
  
         Por exemplo; * * ARRConfig config cert ***c:\temp\certificate.pfx*** hostnames ***mail.contoso.com*** servidor de destino * * * ExchangeSvr * * *  
  
         Substitua *mail.contoso.com* com o nome do seu domínio. Substitua *ExchangeSvr* com o nome do seu servidor que está executando o Exchange Server.  
  
9. Quando solicitado, digite a senha para o certificado.  
  
> [!NOTE]
>  -   Os nomes de host que você fornece devem ser contidos no certificado SSL que você comprou para Exchange Server.  
> -   Se você tiver vários nomes de host, use uma vírgula (,) para separá-los.  
  
 Para verificar se a configuração está funcionando, tente acessar o site OWA para o servidor que está executando o Exchange Server (https://mail. *NomedoSeuDomínio*.com/owa) de um computador que não é um membro do domínio. Para solucionar problemas de conectividade, você também pode usar o online [analisador de conectividade remota da Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=249455) ferramenta.  
  
### <a name="configure-split-dns-for-exchange-server"></a>Configurar dividir o DNS para Exchange Server  
  
> [!NOTE]
>  Essa é uma tarefa recomendada.  
  
 DNS de divisão permite que você configure endereços IP diferentes no DNS para o mesmo nome de host, dependendo de onde a solicitação DNS foi originada. Se o computador cliente é na intranet, a solicitação DNS será resolvido para um endereço IP da intranet. Se o computador cliente estiver na Internet, a solicitação DNS será resolvido para um endereço IP. Isso é transparente para os usuários.  
  
 Recomendamos que você configure divisão de serviços DNS de forma que permite que os usuários sempre usar o mesmo nome de host para acessar o servidor do Exchange, independentemente de seu local.  
  
##### <a name="to-configure-split-dns-for-exchange-server"></a>Para configurar o DNS de divisão para Exchange Server  
  
1.  Faça logon no Windows Server Essentials como administrador e, em seguida, abra o Gerenciador DNS.  
  
2.  Na árvore de console do Gerenciador DNS, clique com botão direito seu servidor e, em seguida, clique em **nova zona**. O **Assistente de nova zona** aparece.  
  
3.  Sobre o **tipo de zona** página do assistente, aceite a opção padrão e, em seguida, clique em **próxima**.  
  
4.  Sobre o **o escopo de replicação de zona do Active Directory** página, aceite a opção padrão e, em seguida, clique em **próxima**.  
  
5.  Sobre o **zona ou inversa pesquisa** página, aceite ou selecione **zona de pesquisa direta**e, em seguida, clique em **próxima**.  
  
6.  Sobre o **nome do fuso** página, digite o FQDN do servidor que está executando o servidor do Exchange (por exemplo; *mail.contoso.com*) e, em seguida, clique em **próxima**.  
  
7.  No **atualização dinâmica** página, aceite a opção padrão, clique em **próxima**e clique em **concluir**.  
  
8.  Na árvore de console do Gerenciador DNS, clique com botão direito a nova zona de pesquisa direta e clique em **novo Host (A ou AAAA)**.  
  
9. Sobre o **novo Host** página, deixe o **nome** campo em branco, digite o endereço IP da intranet do seu servidor que está executando o Exchange Server e, em seguida, clique em **Adicionar Host**.  
  
    > [!NOTE]
    >  Quando você deixar o **nome** campo em branco, o servidor usa o nome do domínio pai por padrão.  
  
10. Sobre o **novo Host** página, clique em **feito**.  
  
> [!NOTE]
>  Se você usa o ActiveSync, mas não é possível sincronizar o email para algumas contas de caixa de correio, determine se essas contas são membros de um ou mais grupos protegidos, como os administradores de domínio. Para obter informações relacionadas que podem ajudá-lo a resolver esse problema, consulte [Exchange ActiveSync retornado um erro de HTTP 500](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx).  
  
## <a name="related-topics"></a>Tópicos relacionados  
 Para obter mais informações sobre como integrar um servidor do Exchange no local, veja as seções a seguir.  
  
### <a name="what-happens-if-i-disable-exchange-integration"></a>O que acontece se eu desativar a integração do Exchange?  
 Se você desabilitar a integração com um servidor do Exchange no local, você estará mais apto a usar o Windows Server Essentials Dashboard para exibir, criar ou gerenciar caixas de correio do Exchange Server.  
  
### <a name="what-do-i-need-to-know-about-email-accounts"></a>O que é necessário saber sobre contas de email?  
 Uma solução de email hospedadas é configurada no seu servidor. Uma solução de um provedor de email hospedadas, como o Microsoft Office 365, pode fornecer contas de email individual para os usuários da rede. Quando você executa a adicionar um Assistente de conta de usuário no Windows Server Essentials para criar uma conta de usuário, o assistente tenta adicionar a conta de usuário para a solução de email hospedadas disponíveis. Ao mesmo tempo, o assistente atribui um nome de email (alias) para o usuário e define o tamanho máximo da caixa de correio (cota). O tamanho máximo da caixa de correio varia dependendo do provedor de email que você usa. Depois de adicionar a conta de usuário, você pode continuar a gerenciar as informações de alias e cota de caixa de correio na página de propriedades para o usuário. Para gerenciamento completo de suas contas de usuário e o provedor de email hospedada, use o console de gerenciamento de seu provedor hospedado. Dependendo do seu provedor, você pode acessar o console de gerenciamento de um portal baseado na web ou de uma guia no painel do servidor.  
  
 O alias que você fornece quando você executa a adicionar um Assistente de conta de usuário é enviado para o provedor de email hospedada como o nome sugerido para o alias do usuário. Por exemplo, se o alias do usuário é *FrankM*, o endereço de email do usuário s pode ser * FrankM@Contoso.com *.  
  
 Além disso, a senha que você definiu para o usuário na adicionar um Assistente de conta de usuário será a senha do usuário na solução email hospedado inicial.  
  
 Por fim, se você excluir o usuário usando a excluir um Assistente de conta de usuário no servidor, o assistente também envia uma solicitação ao provedor de email hospedadas para excluir o usuário de seu sistema também. O provedor pode excluir a conta de usuário s e o email que está associado à conta.  
  
 Para obter informações do usuário sobre como configurar o software de cliente de e-mail obrigatório ou como acessar uma conta de email, consulte a documentação de ajuda fornecida pelo seu provedor de email hospedado.  
  
### <a name="what-is-a-mailbox-quota"></a>O que é uma cota de caixa de correio?  
 A quantidade de espaço de armazenamento que é alocada para um usuário de rede s dados de caixa de correio do Exchange é conhecida como a cota de caixa de correio.  
  
 Quando você executa o **configurar a integração com o Exchange Server** tarefa no painel, o assistente adiciona uma página para o Assistente de conta de usuário adicionar que permite que você escolher para impor cotas de caixa de correio e para especificar o tamanho da cota. Por padrão, o **impor cotas de caixa de correio** opção é selecionada (on) e caixas de correio do usuário são atribuídas 2 GB de espaço de armazenamento. Os administradores do Exchange podem personalizar as configurações de cota de caixa de correio para atender às necessidades de seus negócios.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Requisitos de sistema do Windows Server Essentials](../get-started/system-requirements.md)  
  
-   [Gerenciar a integração do serviço de Email](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
