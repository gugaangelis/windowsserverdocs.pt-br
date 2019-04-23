---
title: Implantar a Experiência do Windows Server Essentials como um servidor hospedado
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a455c6b4-b29f-4f76-8c6b-1578b6537717
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b44b395a39a53194b73a0d503c2310edcbe53a2c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876067"
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>Implantar a Experiência do Windows Server Essentials como um servidor hospedado

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este documento inclui informações específicas a hosters que pretendem implantar o Microsoft Windows Server 16 com a função experiência Windows Server Essentials (conhecida como Windows Server Essentials no restante do documento) instalada no laboratório e pretende oferecer a experiência do Windows Server Essentials como um serviço aos clientes. Este documento inclui as seguintes seções:  
  

-   [Visão geral da experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Benefícios de hospedagem da experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opções de implantação com suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologias de rede com suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizar a imagem da função de experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizar a implantação de experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrar dados do Windows Small Business Server para a experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Executar tarefas comuns usando o Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integração de email com o Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Monitorar e gerenciar usando ferramentas nativas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Cenários de teste](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informações de suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

-   [Visão geral da experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Benefícios de hospedagem da experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opções de implantação com suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologias de rede com suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalizar a imagem da função de experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizar a implantação de experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrar dados do Windows Small Business Server para a experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Executar tarefas comuns usando o Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integração de email com o Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Monitorar e gerenciar usando ferramentas nativas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Cenários de teste](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informações de suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

  
##  <a name="BKMK_WSEEOverview"></a> Visão geral da experiência do Windows Server Essentials  
 A experiência do Windows Server Essentials é uma função de servidor que está disponível no Windows Server 2012 R2 Standard e Datacenter do Windows Server 2012 R2. Quando a função experiência do Windows Server Essentials é instalada em um servidor executando o Windows Server 2012 R2, o cliente pode tirar proveito de todos os recursos que estão disponíveis no Windows Server Essentials sem os bloqueios e limites. A experiência do Windows Server Essentials permite que as seguintes soluções entre instalações para pequenas e médias empresas:  
  
-   **Armazenamento de dados e proteção** você pode armazenar o cliente "dados do relatório s em um local centralizado e proteger os dados de servidor e cliente, fazendo backup de computadores servidor e cliente (menos de 75) dentro da rede.  
  
-   **Gerenciamento de usuário** Você pode gerenciar os usuários e grupos por meio do painel de servidor simplificado. Além disso, integração com o Microsoft Azure Active Directory (Azure AD) permite acesso fácil aos dados do Microsoft online services (por exemplo, Office 365, Exchange Online e SharePoint Online) para os usuários usando suas credenciais de domínio.  
  
-   **Integração com o serviço** integrar o servidor de serviços online da Microsoft (como o Office 365, SharePoint Online e Backup do Microsoft Azure). Também é possível integrar o servidor com os serviços ou serviços fornecidos por provedores de terceiros.  
  
-   **Acesso em qualquer lugar** O cliente pode acessar o servidor, os computadores da rede e os dados de praticamente qualquer lugar que tiver uma conexão de Internet e usando quase qualquer dispositivo. O Acesso via Web remoto permite que eles acessem aplicativos e dados com uma experiência de navegador simplificada e compatível com toque. O aplicativo My Server permite que eles acessem dados de um Windows Phone ou um aplicativo da Microsoft Store.  
  
-   **Streaming de mídia** se você instalar o pacote de mídia em um servidor com experiência do Windows Server Essentials habilitada, o cliente final pode armazenar músicas, vídeos e fotografias em pastas compartilhadas e acessar esses arquivos de mídia de computadores em rede ou Acesso via Web remoto.  
  
-   **Monitoramento de integridade** Você pode monitorar a integridade da rede e obter relatórios de integridade personalizados.  
  
##  <a name="BKMK_Benefits"></a> Benefícios de hospedagem da experiência do Windows Server Essentials  
  Experiência do Windows Server Essentials é uma função no Windows Server, portanto, você pode reutilizar a estrutura de gerenciamento no Windows Server para implantar e configurar a função experiência Windows Server Essentials e a implantação existente. Que hospeda a função experiência Windows Server Essentials fornece os seguintes benefícios:  
  
-   **Implantação simplificada** ao simplesmente ativar a função experiência do Windows Server Essentials, alguns dos mais usados com frequência as funções e recursos são ativados e configurados com as práticas recomendadas para pequenas e médias empresas. Você pode personalizar os recursos do Windows Server Essentials ou ocultar alguns dos recursos locais. Se você usar o Windows Azure Pack, você pode baixar o modelo da Galeria para a experiência do Windows Server Essentials no Windows Server 2012 R2.  
  
-   **Painel simplificado** O Painel do Windows Server Essentials simplifica tarefas comuns, como o gerenciamento de pastas do servidor, armazenamento de servidor, backup e restauração, usuário ou contas de grupo, dispositivos, acesso remoto e email. Clientes de empresas de pequeno e médio porte podem executar as tarefas diárias de gerenciamento em vez de chamar o Suporte para obter suporte técnico.  
  
-   **Extensibilidade** O Painel do Windows Server Essentials e o software Connector do Windows Server Essentials são extensíveis. Você pode adicionar sua própria marca e integração de serviço, para que os clientes tenham um ponto de entrada para tudo sobre seu servidor e o serviço.  
  
-   **Monitor** Uma nova versão do System Center Monitoring Pack está disponível para monitorar e gerenciar vários servidores que executam o Windows Server Essentials. Para baixar o pacote de gerenciamento, consulte [System Center Management Pack para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
##  <a name="BKMK_SupportedDeployment"></a> Opções de implantação com suporte  
  Experiência do Windows Server Essentials pode ser implantada como um controlador de domínio em um novo ambiente do Active Directory; ou ele pode ser implantado em um ambiente existente do Active Directory como membro do domínio.  
  
 É recomendável que você primeiro implantar o Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter e, em seguida, instale a função experiência do Windows Server Essentials. Com esse método de implantação, você obtém todas as funcionalidades da edição do Windows Server Essentials, sem os bloqueios e limites.  
  

 Para obter mais informações sobre como instalar o Windows Server 2012 R2 com a função experiência Windows Server Essentials, consulte [instalar e configurar o Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  


  
##  <a name="BKMK_SupportedToplogy"></a> Topologias de rede com suporte  
 Para usar a experiência do Windows Server Essentials em um cliente de roaming, o VPN deve ser habilitado. Para habilitar o acesso remoto ao servidor de clientes móveis, você precisará abrir a porta 443 e a porta 80 no servidor.  
  
 Aqui estão duas topologias típicas de rede no lado do servidor e como a VPN e o Acesso via Web remoto podem ser configurados:  
  
-   **Topologia 1** (Esta é a topologia preferencial e ela coloca todos os servidores e o intervalo de IP da VPN na mesma sub-rede):  
  
    -   Configure o servidor em uma rede virtual separada sob um dispositivo de conversão de endereço de rede (NAT).  
  
    -   Habilite o serviço DHCP na rede virtual ou atribua um endereço IP estático para o servidor.  
  
    -   Encaminhe a porta 443 do IP público no roteador para o endereço de rede local do servidor.  
  
    -   Permita a passagem da VPN para a porta 443.  
  
    -   Defina o conjunto de endereços IPv4 da VPN no mesmo intervalo de sub-rede que o endereço do servidor.  
  
    -   Atribua um segundo servidor ao endereço IP estático na mesma sub-rede, mas fora do conjunto de endereço da VPN.  
  
-   **Topologia 2**:  
  
    -   Atribua ao servidor um endereço IP privado.  
  
    -   Permita que a porta 443 no servidor acesse um endereço IP público da porta 443.  
  
    -   Permita a passagem da VPN para a porta 443.  
  
    -   Atribua diferentes intervalos para o conjunto de endereços IPv4 da VPN e o endereço do servidor.  
  
 Com a topologia 2, não há suporte para cenários de segundo servidor porque não é possível adicionar outro servidor ao mesmo domínio.  
  
 Você pode habilitar a VPN durante uma implantação autônoma usando nosso script do Windows PowerShell ou ela pode ser configurada com o assistente após a configuração inicial.  
  
 Para habilitar a VPN usando o Windows PowerShell, execute o seguinte comando com privilégios administrativos no servidor que executa o Windows Server Essentials e forneça todas as informações necessárias.  
  
```  
##  
## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
##  
  
$myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
$mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
$mySslCertificatePassword = ConvertTo-SecureString  œAsPlainText  œForce '******';   ## password for private key of the SSL certificate  
$skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
Set-WssDomainNameConfiguration  œDomainName $myExternalDomainName  œCertificatePath $mySslCertificateFile  œCertificateFilePassword $mySslCertificatePassword  œNoCertificateVerification  
##  
## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
##  
  
Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
  
```  
  
> [!NOTE]
>  Se você não puder fornecer uma conexão VPN antes de o cliente ter o controle do servidor, certifique-se de que a porta 3389 do servidor seja acessível pela Internet para que o cliente possa usar o protocolo RDP para se conectar ao servidor e configure-o.  
  
##  <a name="BKMK_CustomizeImage"></a> Personalizar a imagem da função de experiência do Windows Server Essentials  
 Você pode personalizar a imagem antes de configurar a função da Experiência do Windows Server Essentials. Para saber mais sobre o processo padrão do Windows Server Sysprep, consulte [Kit de Avaliação e Implantação do Windows](https://msdn.microsoft.com/library/hh825420.aspx). Depois de preparar a imagem usando o Sysprep, você pode usá-lo ou selá-lo novamente em Install. wim para uma nova implantação.  
  
 Se estiver usando o Virtual Machine Manager, você pode criar um modelo usando a instância em execução. Esse processo usa o Sysprep para preparar a instância e ele desliga o computador. Depois que você armazená-lo na sua biblioteca, você pode usá-lo caso a caso.  
  
 Depois de instalar a função experiência do Windows Server Essentials, você pode personalizar os recursos no Windows Server Essentials. Uma das personalizações mais importantes é a chave do Registro **IsHosted**: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**.  
  
 Se essa chave estiver definida como 0x1, alguns dos recursos locais terão o comportamento alterado. Essas alterações de recurso incluem:  
  
-   **Backup do cliente** O backup de cliente será desativado por padrão para computadores cliente recentemente associados.  
  
-   **Serviço de restauração do cliente** Serviço de restauração do cliente will be disabled, and the UI will be hidden from the Dashboard.  
  
-   **Histórico de arquivos** As configurações do histórico de arquivos para contas de usuário recém-criadas não serão gerenciadas automaticamente pelo servidor.  
  
-   **Backup do servidor** O serviço de Backup do servidor será desabilitado e a interface do usuário do servidor de Backup ficará oculta do Painel.  
  
-   **Espaços de armazenamento** A interface do usuário para criar ou gerenciar espaços de armazenamento ficará oculta do Painel.  
  
-   **Acesso em qualquer local** A configuração do roteador e VPN será ignorada por padrão quando você executar o Assistente de configuração Acesso em qualquer lugar.  
  
 Se você deseja controlar o comportamento de cada recurso listado, você pode definir a chave do Registro correspondente para cada um deles. Para obter informações sobre como definir a chave do Registro, consulte [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
##  <a name="BKMK_AutomateDeployment"></a> Automatizar a implantação de experiência do Windows Server Essentials  
 Para automatizar a implantação, você precisa primeiro implantar o sistema operacional e, em seguida, instale a função experiência do Windows Server Essentials.  
  
-   Para implantar automaticamente o Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter, siga as instruções em [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx).  
  
-   Para saber como instalar a função experiência do Windows Server Essentials usando o Windows PowerShell, consulte [instalar e configurar o Windows Server Essentials](https://technet.microsoft.com/library/dn281793.aspx).  
  
> [!NOTE]
>  Certifique-se de que as configurações de fuso horário da máquina virtual do host e a experiência do Windows Server Essentials são os mesmos. Caso contrário, você pode enfrentar vários erros. Eles incluem: a configuração inicial do servidor pode não ser bem-sucedida em tarefas relacionadas do certificado, o certificado pode não funcionar por algumas horas após a função experiência Windows Server Essentials instalada, e informações do dispositivo não serão atualizado corretamente.  
  
 Após a implantação, use o cmdlet do Windows PowerShell **Get-WssConfigurationStatus** para verificar se a configuração inicial foi bem-sucedida. O status retornado deve ser um dos seguintes: **Notstarted**, **FinishedWithWarning**, **Executando**, **Concluído**, **Falha** ou **PendingReboot**.  
  
 O servidor será reiniciado durante a configuração inicial. Se você precisa impedir esta reinicialização automática, você pode usar o seguinte comando para adicionar uma chave do Registro antes de iniciar a configuração inicial:  
  
```  
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false  
  
```  
  
 Depois que a configuração inicial começar, vocé poderá usar **Get-WssConfigurationStatus** para verificar o status da configuração inicial, e, quando o status for **PendingReboot**, poderá reiniciar o servidor.  
  
##  <a name="BKMK_Migrate"></a> Migrar dados do Windows Small Business Server para a experiência do Windows Server Essentials  
 Você pode migrar dados de servidores que executam o Windows Small Business Server 2011, Windows Small Business Server 2008, Windows Small Business Server 2003 ou Windows Server Essentials para o servidor que executa o Windows Server Essentials. Examine os [migrar para o Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md) migração guia para 2migrations local e faça as personalizações necessárias com base em seu ambiente de hospedagem.  
  
> [!NOTE]
>  Recomenda-se colocar o servidor de origem e o servidor de destino na mesma sub-rede. Se isso não for possível, você deve garantir que:  
>   
>  -   O servidor de origem e o servidor de destino podem acessar uns aos outros "relatório s nomes DNS internos.  
> -   Todas as portas necessárias estejam abertas.  
  
 Após a migração, você pode atualizar suas licenças para remover os bloqueios e limites. Para obter mais informações, consulte [transição do Windows Server Essentials para o Windows Server 2012 Standard](https://technet.microsoft.com/library/jj247582.aspx).  
  
##  <a name="BKMK_PowerShell"></a> Executar tarefas comuns usando o Windows PowerShell  
 Esta seção explica algumas das tarefas comuns que você pode executar usando o Windows PowerShell.  
  
### <a name="enable-remote-web-access"></a>Habilitar Acesso Remoto da Web  
 **Sintaxe**:  
  
 Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
  
 **Exemplo**:  
  
 $Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
  
 Esse comando permitirá Acesso Remoto da Web com o roteador configurado automaticamente, e trocará as permissões de acesso padrão para todos os usuários existentes.  
  
### <a name="add-user"></a>Adicionar usuário  
 **Sintaxe**:  
  
 Adicionar-WssUser [-nome] < cadeia de caracteres\> [-senha] < securestring\> [-AccessLevel < cadeia de caracteres\> {usuário &#124; administrador}] [-FirstName < cadeia de caracteres\>] [-LastName < cadeia de caracteres\>] [- AllowRemoteAccess] [-AllowVpnAccess] [< CommonParameters\>]  
  
 **Exemplo**:  
  
 $password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
  
 Esse comando adicionará um administrador chamado User2Test com senha Passw0rd!.  
  
### <a name="add-server-folder"></a>Adicionar Pasta do Servidor  
 **Sintaxe**:  
  
 Adicionar-WssFolder [-nome] < cadeia de caracteres\> [-caminho] < cadeia de caracteres\> [[-descrição] < cadeia de caracteres\>] [-KeepPermissions] [< CommonParameters\>]  
  
 **Exemplo**:  
  
 $Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
  
 Esse comando adicionará uma pasta de servidor chamada MyTestFolder no local especificado.  
  
##  <a name="BKMK_EmailIntegration"></a> Integração de email com o Windows Server Essentials  
 Você pode integrar a experiência do Windows Server Essentials com o Office 365 ou Exchange Server hospedado. Se quiser que o cliente use o email hospedado, precisará desenvolver um complemento para integrar a Experiência do Windows Server Essentials com sua solução de email hospedada. Para obter mais informações, consulte a [SDK do Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx).  
  
##  <a name="BKMK_Monitoring"></a> Monitorar e gerenciar usando ferramentas nativas  
 Esta seção discute as ferramentas nativas que estão disponíveis no Windows Server 2012 R2 para monitorar e gerenciar o servidor.  
  
### <a name="group-policy"></a>Política de Grupo  
  A experiência do Windows Server Essentials aproveita o suporte nativo de política de grupo no Windows Server 2012 R2 e fornece uma interface de usuário para definir as configurações de segurança e redirecionamento de pasta.  
  
> [!NOTE]
>  Em um ambiente hospedado, se o Redirecionamento de pasta para um perfil de usuário estiver habilitado, ele pode aumentar o tempo para o usuário final entrar quando o tamanho dos dados for grande.  
  
### <a name="system-center-monitoring-pack"></a>Pacote de monitoramento do System Center  
 Pacote de monitoramento do System Center para monitores de experiência do Windows Server Essentials, o sistema de alerta de integridade para ajudá-lo a gerenciar um grande número de servidores que executam o Windows Server Essentials que são dedicados a organizações de pequenas empresas. Para obter mais informações, consulte [System Center Management Pack para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
### <a name="backup-and-restore"></a>Backup e restauração  
  Windows Server 2012 R2 com a experiência do Windows Server Essentials permite que você faça backup do servidor e computadores cliente na rede.  
  
#### <a name="server-backup"></a>Backup do servidor  
 O Windows Server Essentials oferece suporte a duas maneiras de fazer backup do servidor: backup local e backup externo. Se você deseja implantar sua própria solução de backup do servidor, você pode personalizar essas opções.  
  
-   O**backup local** permite que você realize backup incremental no nível do bloco regularmente em um disco separado. Como hoster, você pode anexar um disco rígido virtual à máquina virtual que executa o Windows Server Essentials e, em seguida, configurar o backup do servidor nesse disco rígido virtual. O disco rígido virtual deve estar localizado em um disco físico diferente da máquina virtual que executa o Windows Server Essentials.  
  
    > [!NOTE]
    >  Se você tiver outras soluções de backup para as máquinas virtuais e não quiser que os usuários vejam o recurso de Backup do servidor nativo do Windows Server Essentials, você pode desativá-lo e remover a interface do usuário relacionada do Painel. Para obter mais informações, consulte a seção [Personalizar backup do servidor](https://technet.microsoft.com/library/dn293413.aspx) de [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
-   **Backup externo** Permite periodicamente fazer backup de dados do servidor para um serviço baseado em nuvem. Você pode baixar e instalar o Microsoft Azure Backup Integration Module para Windows Server Essentials para aproveitar o Backup do Azure que é fornecida pela Microsoft.  
  
     Para obter mais informações, consulte a integrar o Windows Azure Backup com a seção do Windows Server Essentials no [Manage Server Backup](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md).  
  
     Se você ou seus usuários preferirem outro serviço de nuvem, você deve:  
  
    -   Atualize a interface do usuário do painel para que ele se vincula ao seu serviço de nuvem preferido, em vez de para o padrão de Backup do Azure.  
  
    -   (Opcional) Desenvolva um complemento para o Painel para configurar e gerenciar o serviço de backup baseado em nuvem.  
  
#### <a name="client-computer-backup"></a>Backup do Computador Cliente  
 O Experiência do Windows Server Essentials oferece suporte a dois tipos de backup de dados do cliente: backup do cliente completo e histórico de arquivos.  
  
> [!NOTE]
>  Realizar o backup do cliente pode ter impacto sobre o desempenho, pois os dados precisam ser transferidos do cliente para o servidor por VPN.  
  
##### <a name="full-client-backup"></a>Backup completo do cliente  
 O backup completo do cliente é ativado por padrão para todos os dispositivos de cliente conectados à rede Windows Server Essentials. Ele faz backup completo das informações de sistema e dados para o cliente e oferece suporte à eliminação de duplicação de dados. Os dados de backup serão armazenados no servidor executando o Windows Server Essentials. Isso permite que um cliente com falha recupere dados de um ponto de backup anterior.  
  
 Algumas considerações para o backup de computador completo do cliente são:  
  
-   **Desempenho** O backup inicial do cliente pode ser demorado em função da quantidade de dados a serem carregados.  
  
-   **Estabilidade** Às vezes, a conexão com a Internet não é estável no lado do cliente. Backup do cliente é projetado para retomar automaticamente e o banco de dados de backup do cliente cria um ponto de verificação sempre que 40 GB de dados é feito backup. Você pode alterar esse valor para um número menor se esperar que a conexão com a Internet seja pouco confiável.  
  
    -   Para habilitar uma tarefa de ponto de verificação: No servidor, defina a chave do Registro **HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs** como 1.  
  
    -   Para alterar o limite do ponto de verificação: No cliente, altere **HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold** do valor padrão de 40 GB.  
  
-   **Restauração Bare Metal do Cliente** Como o Ambiente de Pré-instalação do Windows não tem suporte para conexão VPN, não há suporte para a Restauração Bare Metal do Cliente. Você deve ocultar a tarefa de Serviço de Restauração do Cliente seguindo as etapas em [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##### <a name="file-history"></a>Histórico de Arquivos  
 Histórico de arquivos é um recurso no Windows 8.1 e Windows 8 para fazer backup dos dados de perfil (bibliotecas, área de trabalho, contatos, favoritos) para um compartilhamento de rede. Você pode gerenciar centralmente a configuração do Histórico de arquivos de todos os computadores que executam o Windows 8.1 ou Windows 8 que fazem parte da rede do Windows Server Essentials. Os dados de backup são armazenados no servidor executando o Windows Server Essentials. Você deve ocultar a tarefa de Serviço de restauração do cliente seguindo as etapas em [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
### <a name="storage-management"></a>Gerenciamento de armazenamento  
 Os espaços de armazenamento permitem agregar a capacidade de armazenamento físico de discos rígidos diferentes, incluir dinamicamente discos rígidos e criar volumes de dados com níveis especificados de resiliência. Você pode fazer isso no host ou na máquina virtual. Se você quiser ocultar esse recurso em uma máquina virtual que executa o Windows Server Essentials, siga as instruções em [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##  <a name="BKMK_Scenarios"></a> Cenários de teste  
 Da perspectiva de hospedagem, recomendamos que você teste os seguintes cenários:  
  

-   [Implantação de servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)  
  
-   [Configuração do servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)  
  
-   [Gerenciamento de servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)  
  
-   [Experiência do cliente](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)  

  
###  <a name="BKMK_ServerDeploy"></a> Implantação de servidor  
 Você pode testar os seguintes cenários de implantação do servidor:  
  
-   Implantar um servidor executando o Windows Server 2012 R2 como um controlador de domínio em seu ambiente de laboratório e, em seguida, instale a função experiência do Windows Server Essentials.  
  
-   Implantar um servidor executando o Windows Server 2012 R2 no seu ambiente de laboratório, associe esse servidor a um domínio existente e, em seguida, instale a função experiência do Windows Server Essentials.  
  
-   Personalize a imagem do Windows Server Essentials conforme necessário.  
  
-   Automatize a implantação do Windows Server Essentials com arquivo autônomo e Windows PowerShell.  
  
-   Migre servidores locais que executam o Windows Small Business Server para servidores hospedados que executam o Windows Server Essentials.  
  
###  <a name="BKMK_ServerConfig2"></a> Configuração do servidor  
 Você pode testar os seguintes cenários de configuração do servidor:  
  
-   Configure o Acesso em qualquer lugar (rede privada virtual, Acesso via Web remoto e DirectAccess).  
  
-   Configurar o Armazenamento e as Pastas do Servidor.  
  
-   Ative o BranchCache.  
  
-   (Se aplicável) Configure Backup de Servidor, Backup Online, Backup do Cliente, Histórico de Arquivos.  
  
-   (Se aplicável) Configurar e gerenciar os Espaços de Armazenamento.  
  
-   (Se aplicável) Configure a integração da solução de email (Office 365 e Exchange Server hospedado).  
  
-   (Se aplicável) Configure a integração com outros serviços online da Microsoft.  
  
-   (Se aplicável) Configurar o Servidor de Mídia.  
  
###  <a name="BKMK_ServerManage"></a> Gerenciamento de servidor  
 Você pode testar os seguintes cenários de gerenciamento de servidor:  
  
-   Gerencie usuários e grupos.  
  
-   Configurar e receber notificações de alertas por email.  
  
-   Execute o Analisador de práticas recomendadas para ver se há uma mensagem de erro ou aviso.  
  
-   Configure o pacote do System Center Operations Manager.  
  
-   Configure a recuperação do servidor, em caso de corrupção no sistema operacional.  
  
###  <a name="BKMK_ClientXP"></a> Experiência do cliente  
 Você pode testar os seguintes cenários de usuário final:  
  
-   Implante clientes pela Internet (PC ou sistemas operacionais Mac).  
  
-   Usa a Barra Inicial no cliente para acessar Pastas Compartilhadas.  
  
-   Acesse ativos do servidor por Acesso Remoto da Web de diferentes dispositivos (PC, telefone, tablet).  
  
-   Acesse o aplicativo My Server para Windows Phone.  
  
-   (Se aplicável) Acesse o Histórico de arquivos, Backup e restauração do cliente Redirecionamento de pasta.  
  
-   (Se aplicável) Verifique a experiência de integração de email.  
  
##  <a name="BKMK_Support"></a> Informações de suporte  
 Você pode baixar o Windows Server Essentials Software Development Kit (SDK) e o Windows Server Essentials Assessment e Deployment Kit (ADK):  
  
-   [Software Development Kit do Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx)SDK  
  
-   [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
## <a name="see-also"></a>Consulte também  
  
-   [O que há de novo no Windows Server Essentials](../get-started/what-s-new.md)  

-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)  

-   [Introdução ao Windows Server Essentials](../get-started/get-started.md)
