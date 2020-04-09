---
title: Integrar um Exchange Server local com o Windows Server Essentials [fwlink_WSE_OPE]
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: b56a21e2-c9e3-4ba9-97d9-719ea6a0854b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a18da3e8de1337eb3253725a7c50c1ebed1dc6c7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852869"
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>Integrar um Exchange Server local com o Windows Server Essentials [fwlink_WSE_OPE]

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este guia fornece informações e instruções básicas para ajudá-lo a configurar e integrar um servidor local que executa o Exchange Server com um servidor que executa o Windows Server Essentials.  

 Você deve ler este guia antes de tentar implantar um servidor local que executa o Exchange Server em uma rede do Windows Server Essentials.  

> [!NOTE]
>  O Exchange Server 2010 não oferece suporte à instalação em computadores que executam o Windows Server 2012.  

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}  
 Antes de instalar o Exchange Server em uma rede do Windows Server Essentials, certifique-se de concluir as tarefas descritas nesta seção.  

-   [Configurar um servidor que esteja executando o Windows Server Essentials](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)  

-   [Preparar um segundo servidor no qual instalar o Exchange Server](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)  

-   [Configurar o nome de domínio da Internet](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)  

###  <a name="set-up-a-server-that-is-running-windows-server-essentials"></a><a name="BKMK_SetUpSBS8"></a>Configurar um servidor que esteja executando o Windows Server Essentials  
 Você já deve ter configurado um servidor que está executando o Windows Server Essentials. Ele será o controlador de domínio do servidor que executa o Exchange Server. Para obter informações sobre como configurar o Windows Server Essentials, consulte [Instalar o Windows Server Essentials](../install/Install-Windows-Server-Essentials.md).  

###  <a name="prepare-a-second-server-on-which-to-install-exchange-server"></a><a name="BKMK_SecondServer"></a>Preparar um segundo servidor no qual instalar o Exchange Server  
 Você deve instalar o Exchange Server em outro servidor que esteja executando uma versão do sistema operacional Windows Server com suporte oficial à execução do Exchange Server 2010 ou do Exchange Server 2013. Em seguida, você deve associar o segundo servidor ao domínio do Windows Server Essentials.  

 Para obter informações sobre como unir um segundo servidor ao domínio do Windows Server Essentials, consulte unir um segundo servidor à [rede em conectar](../use/Get-Connected-in-Windows-Server-Essentials.md)-se.  

> [!NOTE]
>  A Microsoft não oferece suporte à instalação do Exchange Server em um servidor que executa o Windows Server Essentials.  

###  <a name="configure-your-internet-domain-name"></a><a name="BKMK_DomainNames"></a>Configurar o nome de domínio da Internet  
 Para integrar o servidor local que executa o Exchange Server ao Windows Server Essentials, é necessário registrar um nome de domínio da Internet válido para sua empresa (como *contoso.com*). Você também deve trabalhar com o provedor de nomes de domínio para criar os registros de recursos de DNS exigidos pelo Exchange Server.  

 Por exemplo, caso o nome de domínio da Internet de sua empresa seja contoso.com e você deseje usar um FQDN (nome de domínio totalmente qualificado) *mail.contoso.com* para fazer referência ao servidor local executando o Exchange Server, trabalhe com o provedor de nomes de domínio para criar os registros de recursos de DNS na tabela a seguir.  


| Nome do registro de recurso |     Tipo de registro     |                                                                         Configuração de registro                                                                          |                                                                                                                                                                                                                                                              Descrição                                                                                                                                                                                                                                                              |
|----------------------|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         email         |      host (A)       |                                                        Endereço=*endereço IP público atribuído pelo ISP*                                                         |                                                                                                                                                                                                   O Exchange Server receberá o email enviado para mail.contoso.com.<br /><br /> Você pode usar um nome diferente, conforme desejar.                                                                                                                                                                                                    |
|          MX          | trocador de emails (MX) |                                            Nome do host=@<br /><br /> Endereço=mail.contoso.com<br /><br /> Preferência=0                                             |                                                                                                                                                                                                      Fornece roteamento de mensagens de email para que email@contoso.com cheguem em seu servidor local que esteja executando o Exchange Server.                                                                                                                                                                                                       |
|         SPF          |     texto (TXT)      |                                                                        v=spf1 a mx ~all                                                                         |                                                                                                                                                                                                                      Registro de recurso que ajuda a evitar que os emails enviados do servidor sejam identificados como spam.                                                                                                                                                                                                                      |
|  autodiscover._tcp   |    serviço (SRV)    | Serviço: _autodiscover<br /><br /> Protocolo: _tcp<br /><br /> Prioridade: 0<br /><br /> Peso: 0<br /><br /> Porta: 443<br /><br /> Host de destino: mail.contoso.com | Permite que o Microsoft Office Outlook e os dispositivos móveis descubram automaticamente o servidor local executando o Exchange Server.<br /><br /> **Observação:** Você também pode configurar um registro de recurso de host de descoberta automática (A) e apontar o registro para o endereço IP público do servidor local que está executando o Exchange Server. Porém, caso implementa essa opção, você também deverá fornecer o certificado SSL SAN (nome alternativo da entidade) que ofereça suporte aos nomes de domínio mail.contoso.com e a autodiscover.contoso.com. |

> [!NOTE]
>  -   Substitua as instâncias de *contoso.com* neste exemplo pelo nome de domínio da Internet registrado.  

 Você deve selecionar um FQDN diferente para o servidor local que executa o Exchange Server em relação ao FQDN usado para o servidor que executa o Windows Server Essentials. Por exemplo, é possível usar *remote.contoso.com* como o FQDN utilizado pelos computadores para acessar o servidor que executa o Windows Server Essentials pela Internet. Você pode usar *mail.contoso.com* como o FQDN utilizado para encaminhar emails para o servidor local que executa o Exchange Server.  

## <a name="install-exchange-server"></a>Instalar o Exchange Server  
 O recurso de integração com Exchange Server no Windows Server Essentials oferece suporte às seguintes versões do Exchange Server:  

- Exchange Server 2013  

- Exchange Server 2010 com Service Pack 1 (SP1)  

  Antes de instalar o Exchange Server no segundo servidor, primeiro é necessário adicionar a conta do administrador local ao grupo **Enterprise Admins**.  

#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>Para adicionar a conta do administrador atual ao grupo Administradores de Empresa  

1.  Faça logon no Windows Server Essentials como administrador.  

2.  Execute o Windows PowerShell como administrador.  

3.  No prompt de comando do Windows PowerShell, digite **Add-ADGroupMember "Enterprise Admins" $env: username**e pressione Enter.  

#### <a name="to-install-exchange-server"></a>Para instalar o Exchange Server  

1.  Faça logon no segundo servidor como administrador.  

2.  Abra o navegador da Internet e navegue até o site do [Assistente de Implantação do Exchange Server](https://go.microsoft.com/fwlink/p/?LinkID=249163) .  

3.  Clique em **On-Premises Only**.  

4.  Clique na opção de nova instalação da versão do Exchange Server que será instalada.  

    > [!NOTE]
    >  Caso esteja migrando de uma instalação do Windows Small Business Server, você deverá selecionar a opção de atualização correta que abrange as etapas de migração.  

5.  Na página seguinte, aceite as configurações padrão e clique em **Avançar**.  

    > [!NOTE]
    >  Caso planeje usar pastas públicas na nova instalação do Exchange Server, altere a configuração para **Sim**.  

6.  Siga as instruções passo a passo na lista de verificação para implantar o Exchange Server.  

     O Assistente de Implantação do Exchange Server também permite:  

    -   Imprimir uma cópia da lista de verificação.  

    -   Enviar uma cópia da lista de verificação para um destinatário de email.  

    -   Baixar a lista de verificação como um arquivo PDF.  

> [!NOTE]
> - Você deve sempre optar pela instalação das Ferramentas de Gerenciamento no servidor que esteja executando o Exchange Server. Elas são exigidas pelo recurso de integração do Exchange Server no Windows Server Essentials.  
>   -   Caso precise configurar diretórios virtuais, recomendamos configurar a propriedade **InternalUrl** de modo que tenha a mesma URL da propriedade **ExternalUrl** para cada diretório virtual. Para obter mais informações, consulte o artigo sobre [gerenciamento de diretórios virtuais do servidor de acesso ao cliente](https://go.microsoft.com/fwlink/p/?LinkId=251058) no site da ajuda online do Exchange Server 2010.  
>   -   Caso queira acessar o OWA (Outlook Web Access) a partir do site de Acesso via Web Remoto no Windows Server Essentials, você deverá definir a propriedade de URL externa para o OWA.  

 Caso esteja realizando uma instalação limpa do Exchange Server 2010, você também poderá usar os scripts a seguir para configurar o Exchange Server.  

#### <a name="to-use-scripts-to-set-up-exchange-server"></a>Para usar scripts de configuração do Exchange Server  

1.  Abra o Bloco de Notas e cole o seguinte script em um arquivo novo:  

```powershell
Import-Module ServerManager

Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  Restart
```

2. Salve o arquivo como **InstallDependencies.ps1**.  

3. Copie o certificado SSL do Exchange em um local no servidor.  

4. Abra um novo arquivo do Bloco de Notas e copie o seguinte texto nele:  

```powershell
param (
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]
    $CertPath = "c:\certificates\ExchangeCertificate.pfx",
    [Security.SecureString]
    [Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]
    $CertPassword = $null,
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]
    $DomainName = "contoso.com",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]
    $ServerIpAddress = "192.168.0.1",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]
    $InternalIpRange = "192.168.0.0-192.168.0.255"
)

#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.

Import-ExchangeCertificate  -FileData ([Byte[]]$(Get-content -Path $CertPath  -Encoding byte  -ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force

#New AcceptedDomain and set it to default

New-AcceptedDomain  -Name "official name"  -DomainName $domainname

Set-AcceptedDomain  -Identity "official name"  -MakeDefault $true

#New EmailAddress Policy

$address = "%m@" + $DomainName

New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address

#Set owa and ecp VirtualDirectory ExternalUrl

$hostname = "mail." + $DomainName

$owa = "https://" + $hostname + "/owa"

$ecp = "https://" + $hostname + "/ecp"

$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"

$oab = "https://" + $hostname + "/OAB"

$ews = "https://" + $hostname + "/EWS/Exchange.asmx"

Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  -ExternalUrl $owa  -InternalUrl $owa

Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  -ExternalUrl $ecp  -InternalUrl $ecp

Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  -InternalUrl $activesync

Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true

Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force

#Enable outlook Anywhere

Enable-OutlookAnywhere  -ClientAuthenticationMethod:Basic  -ExternalHostname:$hostname  -SSLOffloading:$false

#new receive/send connector

$machinename = get-content env:computername

$bindingIpaddress = $ServerIpAddress + ":25"

$ReceiveConnectorName = $machinename + "\Default " + $machinename

Set-ReceiveConnector $ReceiveConnectorName -RemoteIPRanges $InternalIpRange

New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated

New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename
```

5. Defina os parâmetros no início do script para refletir seu ambiente de rede.  

6. Salve o arquivo como **ConfigureExchange.ps1**.  

7. Execute o Windows PowerShell como administrador.  

8. No prompt de comando do Windows PowerShell, digite **Set-ExecutionPolicy RemoteSigned** e pressione Enter.  

9. Execute o script **InstallDependencies.ps1**.  

10. Reinicie o servidor e execute o Windows PowerShell como administrador.  

11. No prompt de comando do Windows PowerShell, execute o seguinte script:  

    `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`  

   > [!NOTE]
   >  Certifique-se de digitar o caminho correto no programa de instalação do Exchange Server.  

12. Quando a instalação do Exchange Server for concluída, abra o Shell de Gerenciamento do Exchange como administrador.  

13. No prompt de comando do Shell de Gerenciamento do Exchange, digite **Set-ExecutionPolicy RemoteSigned** e pressione Enter.  

14. Execute o script **ConfigureExchange.ps1**.  

15. Reinicie o servidor.  

> [!NOTE]
>  Se você decidir usar um certificado SSL confiável publicamente em vez de um certificado emitido por conta própria, você poderá seguir as instruções no guia de instalação para criar uma solicitação de certificado e enviá-la para a autoridade de certificação selecionada. Você também pode usar o cmdlet de Powershell do Exchange para criar uma solicitação de certificado. Veja um exemplo.  
>   
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`  
>   
>  Personalize os parâmetros do script de modo que reflitam seu ambiente de rede.  

## <a name="post-installation-tasks"></a>Tarefas de pós-instalação  
 Esta seção descreve as tarefas de configuração do servidor que podem ser necessárias na fase pós-instalação que contém informações específicas de configuração de um servidor local que executa o Exchange Server em uma rede do Windows Server Essentials.  

### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>Adicionar o domínio de email público e configurar as políticas de endereço de email  

> [!NOTE]
>  Esta é uma tarefa obrigatória em uma instalação limpa. Ignore-a caso esteja migrando do Windows Small Business Server.  

 Você deve especificar seu domínio de email para que seja o domínio aceito como padrão e configurar a política de endereço de email.  

##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>Para adicionar seu domínio de email como domínio aceito por padrão  

1.  Siga as instruções no artigo [Criar um Domínio Aceito](https://go.microsoft.com/fwlink/p/?LinkId=249174) do Exchange Server para adicionar um domínio aceito.  

2.  Faça logon no segundo servidor como administrador, abra o Console de Gerenciamento do Exchange e navegue até a guia **Transporte de Hub** da **Configuração da Organização**.  

3.  No painel de trabalho do Console de Gerenciamento do Exchange, clique com o botão direito no novo domínio aceito e clique em **Definir como Padrão**.  

4.  Siga as instruções no artigo [Criar uma Política de Endereço de Email](https://go.microsoft.com/fwlink/p/?LinkId=249179) do Exchange Server para criar uma nova política de endereço de email. Você pode aceitar todos os valores padrão, exceto o endereço de email. Para endereço de email, especifique o domínio de email público.  

### <a name="create-smtp-send-and-receive-connectors"></a>Crie conectores de Envio e Recebimento de SMTP  

> [!NOTE]
>  Esta é uma tarefa obrigatória.  

 Você deve configurar um conector de envio SMTP e um conector de recebimento SMTP para transmissão de saída/entrada de mensagens de email.  

 Para criar um conector de envio SMTP, siga as instruções no artigo [Criar um Conector de Envio SMTP](https://technet.microsoft.com/library/aa997285.aspx)do Exchange Server.  

 Para criar um conector de recebimento SMTP, siga as instruções no artigo do Exchange Server sobre [criação de um conector de recebimento SMTP](https://technet.microsoft.com/library/bb125159.aspx).  

 Como opção, você pode consultar o script mostrado anteriormente neste documento para criar os conectores de envio e recebimento usando cmdlets de Powershell do Exchange.  

### <a name="configure-the-network-router"></a>Configurar o roteador de rede  

> [!NOTE]
>  Esta é uma tarefa obrigatória em uma instalação limpa. Se você estiver migrando do Windows Small Business Server, consulte [Migrar dados do servidor para o Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md) para obter instruções sobre como configurar a rede.  

 Você deve definir no mínimo as seguintes configurações de porta no roteador:  

|Porta do roteador|IP de destino|Porta de destino|Observação|  
|-----------------|--------------------|----------------------|----------|  
|25 (SMTP)|IP interno do servidor local que executa o Exchange Server.|25||  
|80 (HTTP)|IP interno do servidor que executa o Windows Server Essentials|80||  
|443 (HTTPS)|IP interno do servidor que executa o Windows Server Essentials|443||  

 Caso a rede ofereça suporte aos protocolos de mensagens POP3 ou IMAP, você também deverá configurar o encaminhamento de porta para esses protocolos. Para obter informações relacionadas, consulte a seção **Servidores de Acesso para Cliente** no tópico [Referência de porta de rede do Exchange](https://go.microsoft.com/fwlink/p/?LinkId=250773) , na biblioteca do TechNet do Exchange Server.  

> [!NOTE]
> - Recomendamos configurar os endereços IP estáticos para o servidor que executa o Windows Server Essentials e para o segundo servidor que executa o Exchange Server. Para obter instruções sobre como configurar um endereço IP estático em um computador que executa o Windows Server 2003 ou o Windows Server 2008 R2, consulte [Configurar um endereço IP estático](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx) na Biblioteca TechNet do Windows Server  
> 
>   **Observação:** A configuração do servidor DNS sempre deve apontar para o endereço IP do servidor que está executando o Windows Server Essentials.  
>   -   No roteador, certifique-se de que os endereços IP do servidor que executa o Windows Server Essentials e do servidor que executa o Exchange Server estejam reservados ou fora do intervalo de endereços IP do DHCP.  
>   -   A configuração de roteador nesta seção parte do pressuposto de que você  tem apenas um endereço IP público atribuído pelo ISP (provedor de serviços de Internet). Caso tenha vários endereços IP públicos, você poderá atribuir um endereço IP diferente ao servidor que executa o Windows Server Essentials e ao servidor que executa o Exchange Server e, em seguida, crie encaminhamentos de porta com base nos endereços IP públicos.  

### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>Habilitar integração com Exchange Server local no Windows Server Essentials  

> [!NOTE]
>  Caso esteja migrando de uma instalação do Windows Small Business Server, recomendamos ignorar esta etapa por enquanto e realizá-la após desinstalar a instalação anterior do Exchange Server no servidor de origem.  

 Após a instalação e configuração de um servidor que execute o Exchange Server, você deverá habilitar a integração com Exchange Server local no servidor que executa o Windows Server Essentials.  

##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>Para habilitar a integração com Exchange Server local a partir do Painel  

1.  Faça logon no servidor que executa o Windows Server Essentials como administrador e abra o Painel.  

2.  Na página **Início**, clique na opção de **conexão com serviço de email** e clique na opção de **integração com Exchange Server**.  

3.  No painel de informações, clique na opção de **configuração da integração com Exchange Server**.  

4.  Siga as instruções no assistente.  

### <a name="configure-a-reverse-proxy"></a>Configurar um proxy inverso  

> [!NOTE]
>  Esta é uma tarefa obrigatória se você tiver apenas uma conexão de Internet do seu provedor de serviços da Internet.  

 O Windows Server Essentials e o Exchange Server oferecem suporte a alguns cenários de acesso remoto para usuários de rede. Por exemplo, se você ativar o Acesso em Qualquer Lugar no servidor que executa o Windows Server Essentials, poderá acessar remotamente o site do Acesso via Web Remoto ou usar VPN (rede privada virtual) para conectar-se remotamente à rede do Windows Server Essentials. Para ter acesso remoto a mensagens de email, você deve usar o Outlook em Qualquer Lugar, OWA (Outlook Web Access) ou ActiveSync.  

 Caso o Windows Server Essentials e o servidor que executa o Exchange Server estejam conectados ao mesmo roteador e o provedor de serviços de Internet forneça somente uma conexão de entrada com a Internet, você deverá usar uma solução de proxy inverso para encaminhar tipos diferentes de solicitações de acesso remoto da Internet com base nos nomes dos hosts de destino. Recomendamos a utilização da extensão ARR (Application Request Routing) IIS com suporte da Microsoft como solução de proxy inverso. Para obter mais informações sobre o Application Request Routing IIS, visite o [site do Application Request Routing](https://go.microsoft.com/fwlink/p/?LinkId=249181).  

##### <a name="to-install-and-configure-application-request-routing"></a>Para instalar e configurar o Application Request Routing  

1. Faça logon no Windows Server Essentials como administrador.  

2. Abra o navegador da Internet e navegue até o [site do Application Request Routing](https://go.microsoft.com/fwlink/p/?LinkId=249181).  

3. No site do ARR, clique em **Instalar** e, em seguida, siga as instruções para instalar o ARR.  

   > [!NOTE]
   >  Você deve selecionar o Módulo de Reescrita de URL durante a instalação do ARR.  
   >   
   >  Ao fim da instalação do ARR, é possível que você receba um erro dizendo que a instalação do KB 2589179 para ARR 2.5 não foi realizada com êxito. Ignore-o.  

4. Quando a instalação do ARR for concluída, reinicie o serviço **Gateway de Área de Trabalho Remota**, caso ele não esteja em execução.  

   > [!NOTE]
   >  Após a instalação do ARR, é possível que o serviço **Gateway de Área de Trabalho Remota** pare. Para reiniciar o serviço manualmente, abra a ferramenta administrativa **Serviços** e reinicie o serviço **Gateway de Área de Trabalho Remota**.  

5. [Baixe o KB2732764 para ARR 2.5](https://go.microsoft.com/fwlink/?LinkID=258302)e instale a atualização no servidor que executa o Windows Server Essentials.  

6. Copie o arquivo de certificado SSL para o Exchange Server no servidor que executa o Windows Server Essentials. O arquivo de certificado deve conter a chave privada e estar no formato PFX.  

   > [!NOTE]
   >  Caso esteja usando um certificado emitido por conta própria, siga as instruções no artigo do Exchange Server sobre [exportação de um certificado do Exchange](https://technet.microsoft.com/library/dd351274.aspx) para exportar o certificado.  

7. Dependendo da versão do Windows Server Essentials em execução, realize um dos procedimentos a seguir:  

   -   No Windows Server Essentials: Abra uma janela de comando como administrador e, em seguida, abra o diretório%ProgramFiles%\Windows Server\Bin.  

   -   No Windows Server Essentials: Abra uma janela de comando como administrador e, em seguida, abra o diretório%Windir%\System32\Essentials.  

8. Com base no cenário de instalação, siga uma destas etapas para configurar o ARR:  

   - Caso esteja realizando uma instalação limpa, execute o seguinte comando:  

      **ARRConfig config-** _caminho do certificado para o arquivo de certificado_ **–** _nomes de hostnames para o Exchange Server_  

     > [!NOTE]
     >  Por exemplo; **ARRConfig config-CERT** _c:\temp\certificate.pfx_ **-hostnames** _mail.contoso.com_  
     > 
     >  Substitua *mail.contoso.com* pelo nome de seu domínio protegido pelo certificado.  

   - Caso esteja migrando do Windows Small Business Server, execute o seguinte comando:  

      **ARRConfig config –** _caminho do certificado para o arquivo de certificado_ **-** _nomes de hostnames para o Exchange Server_ **-TargetServer** _Server Name do Exchange Server_  

      Por exemplo; **ARRConfig config-CERT** _c:\temp\certificate.pfx_ **-hostnames** _mail.contoso.com_ **-TargetServer** _ExchangeSvr_  

      Substitua *mail.contoso.com* pelo nome de seu domínio. Substitua *ExchangeSvr* pelo nome do servidor que executa o Exchange Server.  

9. Quando solicitado, digite a senha do certificado.  

> [!NOTE]
> - Os nomes dos hosts fornecidos devem estar contidos no certificado SSL adquirido para o Exchange Server.  
>   -   Caso tenha vários nomes de hosts, use uma vírgula (,) para separá-los.  

 Para verificar se a configuração funciona, tente acessar o site do OWA para o servidor que está executando o Exchange Server (https://mail. *nome_do_domínio*.com/owa) em um computador que não seja membro do domínio. Para solucionar problemas de conectividade, você também pode usar a ferramenta online [Analisador de Conectividade Remota Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=249455) .  

### <a name="configure-split-dns-for-exchange-server"></a>Configurar DNS dividido para o Exchange Server  

> [!NOTE]
>  Esta é uma tarefa recomendada.  

 O DNS dividido permite configurar endereços IP diferentes no DNS para o mesmo nome de host, dependendo da origem da solicitação de DNS. Caso o computador cliente esteja na Intranet, a solicitação de DNS resolverá para um endereço IP da Intranet. Caso o computador cliente esteja na Internet, a solicitação de DNS resolverá para um endereço IP da Internet. Isso fica claro para os usuários.  

 Recomendamos configurar o DNS dividido de modo a permitir que os usuários sempre usem o mesmo nome de host para acessar os serviços do Exchange Server, independentemente de sua localização.  

##### <a name="to-configure-split-dns-for-exchange-server"></a>Para configurar DNS dividido para o Exchange Server  

1.  Faça logon no Windows Server Essentials como administrador e abra o Gerenciador de DNS.  

2.  Na árvore do console do Gerenciador de DNS, clique com o botão direito no servidor e clique em **Nova Zona**. O **Assistente de Nova Zona** é exibido.  

3.  Na página **Tipo de Zona** do assistente, aceite a opção padrão e clique em **Avançar**.  

4.  Na página **Escopo de Replicação de Zona do Active Directory**, aceite a opção padrão e clique em **Avançar**.  

5.  Na página **Zona de pesquisa direta ou inversa**, aceite ou selecione **Zona de pesquisa direta** e clique em **Avançar**.  

6.  Na página **Nome da Zona**, digite o FQDN do servidor que executa o Exchange Server (por exemplo, *mail.contoso.com*) e clique em **Avançar**.  

7.  Na página **Atualização Dinâmica**, aceite a opção padrão, clique em **Avançar** e clique em **Concluir**.  

8.  Na árvore de console do Gerenciador de DNS, clique com o botão direito do mouse na nova zona de pesquisa direta e, em seguida, clique em **Novo Host (A ou AAAA)** .  

9. Na página **Novo Host**, deixe o campo **Nome** em branco, digite o endereço IP da Intranet do servidor que executa o Exchange Server e clique em **Adicionar Host**.  

    > [!NOTE]
    >  Quando você deixa o campo **Nome** em branco, o servidor usa o nome do domínio pai por padrão,  

10. Na página **Novo Host**, clique em **Concluído**.  

> [!NOTE]
>  Caso utilize o ActiveSync mas não possa sincronizar o email para algumas contas de caixa de correio, determine se essas contas são membros de um ou mais grupos protegidos, como Administradores de Domínio. Para obter informações relacionadas que possam ajudar a solucionar o problema, consulte [O Exchange ActiveSync Retornou um Erro HTTP 500](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx).  

## <a name="related-topics"></a>Tópicos relacionados  
 Para obter mais informações sobre como integrar um Exchange Server local, consulte as seções a seguir.  

### <a name="what-happens-if-i-disable-exchange-integration"></a>O que acontece se eu desabilitar a integração com Exchange?  
 Se você desabilitar a integração com um Exchange Server local, não poderá mais usar o Painel do Windows Server Essentials para exibir, criar ou gerenciar caixas de correio do Exchange Server.  

### <a name="what-do-i-need-to-know-about-email-accounts"></a>O que é necessário saber sobre contas de email?  
 Uma solução de email hospedado é configurada no servidor. Uma solução de um provedor de email hospedado, como o Microsoft Office 365, pode fornecer contas de email individuais para usuários de rede. Quando você executa o Assistente Incluir conta de usuário no Windows Server Essentials para criar uma conta de usuário, o assistente tenta adicionar a conta de usuário à solução de email hospedada disponível. Ao mesmo tempo, o assistente atribui um nome de email (alias) ao usuário e define o tamanho máximo da caixa de correio (cota). O tamanho máximo da caixa de correio varia de acordo com o provedor de email que você usa. Depois de adicionar a conta de usuário, você pode continuar a gerenciar as informações de alias e cota da caixa de correio na página de propriedades do usuário. Para gerenciamento completo de suas contas de usuário e provedor de email hospedado, use o console de gerenciamento do provedor hospedado. Dependendo do seu provedor, você pode acessar o console de gerenciamento de um portal baseado na web ou de uma guia no Painel do servidor.  

 O alias fornecido quando você executa o Assistente Incluir conta de usuário é enviado para o provedor de email hospedado como o nome sugerido para o alias do usuário. Por exemplo, se o alias de usuário for *frankm*, o endereço de email do usuário poderá ser <em>FrankM@Contoso.com</em>.  

 Além disso, a senha que você definiu para o usuário no Assistente Incluir conta de usuário será a senha inicial do usuário na solução de email hospedada.  

 Por fim, se você excluir o usuário usando o Assistente Excluir uma conta de usuário no servidor, o assistente também envia uma solicitação ao provedor de email hospedado para excluir também o usuário do seu sistema. O provedor pode excluir a conta do usuário e o email que está associado à conta.  

 Para obter informações sobre como configurar o software de cliente de email necessário ou acessar uma conta de email, consulte a documentação da ajuda fornecida pelo seu provedor de email hospedado.  

### <a name="what-is-a-mailbox-quota"></a>O que é uma cota de caixa de correio?  
 A quantidade de espaço de armazenamento alocada para os dados da caixa de correio do Exchange de um usuário de rede é conhecida como cota da caixa de correio.  

 Quando você executar a tarefa **Configurar a integração com Exchange Server** no Painel, o assistente adiciona uma página do Assistente Incluir conta do usuário que permite que você escolha se deve impor cotas de caixa de correio e especificar o tamanho da cota. Por padrão, a opção **Impor cotas de caixa de correio** é selecionada (ativada) e são atribuídos 2 GB de espaço de armazenamento às caixas de correio do usuário. Os administradores do Exchange podem personalizar as configurações de cota de caixa de correio para atender às necessidades da sua empresa.  

## <a name="see-also"></a>Consulte também  

-   [Requisitos do sistema para o Windows Server Essentials](../get-started/system-requirements.md)  

-   [Gerenciar a integração do serviço de email](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)  

-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
