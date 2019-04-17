---
title: "Implantar a experiência do Windows Server Essentials como um servidor hospedado"
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
ms.openlocfilehash: c299d2b5f3d6b48693c473754a5205a7d26b5d6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>Implantar a experiência do Windows Server Essentials como um servidor hospedado

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este documento inclui informações que são específicas para hosters que pretendem implantar o Microsoft Windows Server 16 com a função de experiência do Windows Server Essentials (chamada de Windows Server Essentials no restante do documento) instalado no seu laboratório e pretende oferecer a experiência do Windows Server Essentials como um serviço para seus clientes. Este documento inclui as seções a seguir:  
  

-   [Visão geral da experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Benefícios de hospedagem de experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opções de implantação com suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologias com suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalize a imagem da função de experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizar a implantação do Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrar dados do Windows Small Business Server para a experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Executar tarefas comuns usando o Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integração de email com o Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Monitorar e gerenciar usando ferramentas nativas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Cenários de teste](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informações de suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

-   [Visão geral da experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Benefícios de hospedagem de experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Opções de implantação com suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologias com suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personalize a imagem da função de experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatizar a implantação do Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrar dados do Windows Small Business Server para a experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Executar tarefas comuns usando o Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Integração de email com o Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Monitorar e gerenciar usando ferramentas nativas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Cenários de teste](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informações de suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

  
##  <a name="BKMK_WSEEOverview"></a>Visão geral da experiência do Windows Server Essentials  
 A experiência do Windows Server Essentials é uma função de servidor que está disponível no Windows Server 2012 R2 Standard e Windows Server 2012 R2 Datacenter. Quando a função de experiência do Windows Server Essentials é instalada em um servidor que executa o Windows Server 2012 R2, o cliente pode tirar proveito dos recursos que estão disponíveis no Windows Server Essentials sem os limites e bloqueios. A experiência do Windows Server Essentials permite as seguintes soluções entre locais para pequenas e médias empresas:  
  
-   **Armazenamento de dados e proteção** você pode armazenar o cliente "¢ dados em um local centralizado e proteger os dados de servidor e cliente, fazer backup de computadores servidor e cliente (menos de 75) dentro da rede.  
  
-   **Gerenciamento de usuário** você pode gerenciar os usuários e grupos por meio do painel de servidor simplificada. Além disso, integração com o Microsoft Azure Active Directory (Azure AD) permite acesso a dados fáceis para serviços online da Microsoft (por exemplo, Office 365, Exchange Online e SharePoint Online) para os usuários usando suas credenciais de domínio.  
  
-   **Integração com o serviço** você pode integrar o servidor com serviços online da Microsoft (como o Office 365, SharePoint Online e Microsoft Azure Backup). Você também pode integrar o servidor com seus serviços ou serviços fornecidos pelos provedores de serviços de terceiros.  
  
-   **Acesso em qualquer lugar** o cliente pode acessar o servidor, os computadores da rede e os dados de praticamente qualquer lugar que tenham uma conexão de Internet e usando praticamente qualquer dispositivo. Acesso via Web remoto permite que eles acessem aplicativos e dados com uma experiência de navegador simplificada e sensível ao toque. O aplicativo meu servidor permite que eles acessem dados de um Windows Phone ou um aplicativo de Microsoft Store.  
  
-   **Streaming de mídia** se você instalar o pacote de mídia em um servidor com o Windows Server Essentials Experience habilitado, o cliente final pode armazenar música, vídeo e fotografias em pastas compartilhadas e acessar esses arquivos de mídia de computadores em rede ou acesso via Web remoto.  
  
-   **Monitoramento de integridade** você pode monitorar a integridade da rede e obter relatórios de saúde personalizadas.  
  
##  <a name="BKMK_Benefits"></a>Benefícios de hospedagem de experiência do Windows Server Essentials  
  A experiência do Windows Server Essentials é uma função no Windows Server, então você pode reutilizar a implantação existente e a estrutura de gerenciamento no Windows Server para implantar e configurar a função de experiência do Windows Server Essentials. A função de experiência do Windows Server Essentials de hospedagem fornece os seguintes benefícios:  
  
-   **Simplificada implantação**, simplesmente ativando a função de experiência do Windows Server Essentials, alguns dos mais comumente usado funções e recursos estão ligados e configurados com as práticas recomendadas para pequenas e médias empresas. Você pode personalizar os recursos do Windows Server Essentials ou ocultar alguns dos recursos locais. Se você usar o Windows Azure Pack, você pode baixar o modelo de galeria de experiência do Windows Server Essentials no Windows Server 2012 R2.  
  
-   **Painel simplificada** o Windows Server Essentials Dashboard simplifica tarefas comuns, como o gerenciamento de pastas de servidor, armazenamento do servidor, backup e restauração, contas de usuário ou grupo, dispositivos, acesso remoto e email. Pequenas e médias empresas podem executar tarefas de gerenciamento diárias em vez de chamar o suporte técnico para obter suporte técnico.  
  
-   **Extensibilidade** o painel do Windows Server Essentials e conector do Windows Server Essentials software são extensíveis. Você pode adicionar sua própria marca e integração, com o serviço para que os clientes tenham um ponto de entrada para tudo sobre seus servidores e o serviço.  
  
-   **Monitor** uma nova versão do System Center monitoramento Pack está disponível para monitorar e gerenciar vários servidores que executam o Windows Server Essentials. Para baixar o pacote de gerenciamento, consulte [System Center Management Pack para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
##  <a name="BKMK_SupportedDeployment"></a>Opções de implantação com suporte  
  Windows Server Essentials Experience pode ser implantado como um controlador de domínio em um novo ambiente do Active Directory; ou ele pode ser implantado em um ambiente existente do Active Directory como membro do domínio.  
  
 Recomendamos que você implantar o Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter primeiro e, em seguida, instale a função de experiência do Windows Server Essentials. Com esse método de implantação, você obtém todas as funcionalidades da edição do Windows Server Essentials, sem os limites e bloqueios.  
  

 Para obter mais informações sobre a instalação do Windows Server 2012 R2 com a função de experiência do Windows Server Essentials, consulte [instalar e configurar o Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  


  
##  <a name="BKMK_SupportedToplogy"></a>Topologias com suporte  
 Para usar o Windows Server Essentials Experience de um cliente móvel, VPN deve ser habilitada. Para habilitar o acesso remoto ao servidor de clientes móveis, você precisa abrir porta 443 e a porta 80 no servidor.  
  
 Aqui estão as topologias de redes de servidor típicas dois e como a VPN e acesso via Web remoto podem ser configurado:  
  
-   **Topologia 1** (Isso é a topologia preferencial, e ela coloca todos os servidores e intervalo VPN IP na mesma sub-rede.):  
  
    -   Configure o servidor em uma rede virtual separado em um dispositivo de conversão de endereços de rede (NAT).  
  
    -   Habilitar o serviço DHCP na rede virtual ou atribuir um endereço IP estático para o servidor.  
  
    -   Encaminhe pública IP porta 443 no roteador para o endereço de rede local do servidor.  
  
    -   Permitir VPN passagem da porta 443.  
  
    -   Defina o pool de endereço IPv4 VPN na mesma faixa de sub-rede como o endereço do servidor.  
  
    -   Servidores segundos atribua um endereço IP estático dentro da mesma sub-rede, mas sem o pool de endereços VPN.  
  
-   **Topologia 2**:  
  
    -   Atribua o servidor um endereço IP privado.  
  
    -   Permitir que a porta 443 no servidor para alcançar um endereço IP pública porta 443.  
  
    -   Permitir VPN passagem da porta 443.  
  
    -   Atribua intervalos diferentes para o pool de endereço IPv4 de VPN e o endereço do servidor.  
  
 Com topologia 2, não há suporte para cenários de servidor segundos porque você não pode adicionar outro servidor no mesmo domínio.  
  
 Você pode habilitar VPN durante uma implantação autônoma usando nosso script do Windows PowerShell, ou ele pode ser configurado com o assistente após a configuração inicial.  
  
 Para habilitar a VPN usando o Windows PowerShell, execute o seguinte comando com privilégios administrativos no servidor que executa o Windows Server Essentials e fornecer todas as informações necessárias.  
  
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
>  Se você não pode fornecer uma conexão de VPN antes que o cliente tomar posse do servidor, certifique-se de que a porta do servidor 3389 está acessível pela Internet para que o cliente pode usar o protocolo RDP para se conectar ao servidor e configurá-lo.  
  
##  <a name="BKMK_CustomizeImage"></a>Personalize a imagem da função de experiência do Windows Server Essentials  
 Você pode personalizar a imagem antes de configurar a função de experiência do Windows Server Essentials. Para saber mais sobre o processo padrão do Windows Server Sysprep, consulte [Kit de avaliação e implantação](https://msdn.microsoft.com/library/hh825420.aspx). Após preparar a imagem usando o Sysprep, você pode usá-lo ou reseal-lo em Install.wim para uma nova implantação.  
  
 Se você estiver usando o Gerenciador de máquina Virtual, você pode criar um modelo usando a instância em execução. Esse processo usa Sysprep para preparar a instância e ele desliga o computador. Depois que você armazená-lo em sua biblioteca, você pode usá-lo caso a caso.  
  
 Depois de instalar a função de experiência do Windows Server Essentials, você pode personalizar os recursos no Windows Server Essentials. Uma das personalizações mais importantes é o **IsHosted** chave do registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**.  
  
 Se essa chave é definida como 0x1, alguns dos recursos locais mudará comportamento. Essas mudanças de recurso incluem:  
  
-   **Backup do cliente** backup do cliente será desativada por padrão em computadores cliente conectados recentemente.  
  
-   **Serviço de restauração de cliente** serviço de restauração do cliente será desabilitado e a interface do usuário do painel ficará oculto.  
  
-   **Histórico de arquivos** configurações do histórico de arquivos para contas de usuário recém-criado não serão automaticamente gerenciadas pelo servidor.  
  
-   **Backup do servidor** serviço de Backup do servidor será desativado e a interface do usuário do servidor de Backup ficará oculto no painel.  
  
-   **Espaços de armazenamento** a interface do usuário para criar ou gerenciar espaços de armazenamento ficará oculto no painel.  
  
-   **Em qualquer lugar acesso** configuração de roteador e VPN será ignorada por padrão quando você executa o Assistente de configuração de qualquer lugar acesso.  
  
 Se você quiser controlar o comportamento de cada recurso listado, você pode definir a chave do registro correspondentes para cada um deles. Para obter informações sobre como definir a chave do registro, consulte o [personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
##  <a name="BKMK_AutomateDeployment"></a>Automatizar a implantação do Windows Server Essentials Experience  
 Para automatizar a implantação, você precisa primeiro implantar o sistema operacional e, em seguida, instale a função de experiência do Windows Server Essentials.  
  
-   Para implantar o Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter automaticamente, siga as instruções em [Kit de avaliação e implantação](https://msdn.microsoft.com/library/hh825420.aspx).  
  
-   Para saber como instalar a função de experiência do Windows Server Essentials por meio do Windows PowerShell, consulte [instalar e configurar o Windows Server Essentials](https://technet.microsoft.com/library/dn281793.aspx).  
  
> [!NOTE]
>  Certifique-se de que as configurações de fuso horário da máquina virtual host e a experiência do Windows Server Essentials sejam os mesmos. Caso contrário, você pode ter vários erros. Isso inclui: a configuração inicial do servidor pode não ter êxito em tarefas relacionadas de certificado, o certificado pode não funcionar para algumas horas depois que a função de experiência do Windows Server Essentials está instalada, e as informações do dispositivo não serão atualizados corretamente.  
  
 Após a implantação, use o cmdlet do Windows PowerShell **Get-WssConfigurationStatus** para verificar se a configuração inicial foi bem-sucedida. O status retornado deve ser uma das seguintes opções: **Notstarted**, **FinishedWithWarning**, **executando**, **concluído**, **Failed**, ou **PendingReboot**.  
  
 O servidor será reiniciado durante a configuração inicial. Se você precisa impedir que a reinicialização automática, você pode usar o seguinte comando para adicionar uma chave do registro antes de começar a configuração inicial:  
  
```  
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false  
  
```  
  
 Depois que a configuração inicial é iniciado, você pode usar **Get-WssConfigurationStatus** para verificar o status de configuração inicial, e quando o status é **PendingReboot**, você pode reiniciar seu servidor.  
  
##  <a name="BKMK_Migrate"></a>Migrar dados do Windows Small Business Server para a experiência do Windows Server Essentials  
 Você pode migrar dados de servidores que executam o Windows Small Business Server 2011, Windows Small Business Server 2008, Windows Small Business Server 2003 ou Windows Server Essentials para o servidor que executa o Windows Server Essentials. Reveja o [migrar para o Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md) migração guia para 2migrations local e faça as personalizações necessárias com base em seu ambiente de hospedagem.  
  
> [!NOTE]
>  Recomendamos que você colocar o servidor de origem e o servidor de destino na mesma sub-rede. Se isso não for possível, você deve garantir que:  
>   
>  -   O servidor de origem e o servidor de destino podem acessar uns aos outros "¢ s internos nomes DNS.  
> -   Todas as portas necessárias estão abertas.  
  
 Após a migração, você pode atualizar suas licenças para remover os limites e bloqueios. Para obter mais informações, consulte [transição do Windows Server Essentials para Windows Server 2012 Standard](https://technet.microsoft.com/library/jj247582.aspx).  
  
##  <a name="BKMK_PowerShell"></a>Executar tarefas comuns usando o Windows PowerShell  
 Esta seção explica algumas das tarefas comuns que você pode executar usando o Windows PowerShell.  
  
### <a name="enable-remote-web-access"></a>Habilitar o acesso via Web remoto  
 **Sintaxe**:  
  
 Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
  
 **Exemplo**:  
  
 $Enable-WssRemoteWebAccess œDenyAccessByDefault œApplyToExistingUsers  
  
 Esse comando habilitar o acesso via Web remoto com o roteador configurado automaticamente e alterar as permissões de acesso padrão para todos os usuários existentes.  
  
### <a name="add-user"></a>Adicionar usuário  
 **Sintaxe**:  
  
 Add-WssUser [-Name] < string\ > [-senha] < securestring\ > [-nível de acesso < string\ > {usuário & #124; Administrador}] [-FirstName < string\ >] [-LastName < string\ >] [-AllowRemoteAccess] [-AllowVpnAccess] [< CommonParameters\ >]  
  
 **Exemplo**:  
  
 $password = ConvertTo-SecureString "Passw0rd!" -asplaintext œforce$ Add-WssUser-nome User2Test-senha $password - nível de acesso Administrador - FirstName User2 - LastName testar  
  
 Esse comando adicionará um administrador denominado User2Test com senha Passw0rd!.  
  
### <a name="add-server-folder"></a>Adicionar pastas de servidor  
 **Sintaxe**:  
  
 Add-WssFolder [-Name] < string\ > [-Path] < string\ > [[-descrição] < string\ >] [-KeepPermissions] [< CommonParameters\ >]  
  
 **Exemplo**:  
  
 $WssFolder adicione-o nome "MyTestFolder"-caminho "C:\ServerFolders\MyTestFolder"  
  
 Esse comando irá adicionar uma pasta de servidor chamada MyTestFolder no local especificado.  
  
##  <a name="BKMK_EmailIntegration"></a>Integração de email com o Windows Server Essentials  
 Você pode integrar a experiência do Windows Server Essentials com o Office 365 ou hospedado Exchange Server. Se você quiser que seu cliente para usar seu email hospedado, você precisa para desenvolver um suplemento para integrar o Windows Server Essentials Experience com sua solução email hospedado. Para obter mais informações, consulte [SDK do Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx).  
  
##  <a name="BKMK_Monitoring"></a>Monitorar e gerenciar usando ferramentas nativas  
 Esta seção discute as ferramentas nativas que estão disponíveis no Windows Server 2012 R2 para monitorar e gerenciar o servidor.  
  
### <a name="group-policy"></a>Política de grupo  
  Windows Server Essentials Experience aproveita o suporte nativo à política de grupo no Windows Server 2012 R2 e fornece uma interface do usuário para definir as configurações de segurança e redirecionamento de pasta.  
  
> [!NOTE]
>  Em um ambiente hospedado, se o redirecionamento de pasta para um perfil de usuário estiver habilitado, ele pode aumentar o tempo para o usuário final fazer logon no quando o tamanho dos dados é grande.  
  
### <a name="system-center-monitoring-pack"></a>Pacote de monitoramento do System Center  
 System Center Monitoring Pack para o sistema de alerta de integridade para ajudá-lo a gerenciar um grande número de servidores que executam o Windows Server Essentials que são dedicados para organizações de pequenas empresas de monitores de experiência do Windows Server Essentials. Para obter mais informações, consulte [System Center Management Pack para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
### <a name="backup-and-restore"></a>Backup e restauração  
  Windows Server 2012 R2 com a experiência do Windows Server Essentials permite fazer backup de servidor e computadores cliente na rede.  
  
#### <a name="server-backup"></a>Backup do servidor  
 Windows Server Essentials oferece duas maneiras para fazer backup do servidor: backup de local e backup fora do local. Se você deseja implantar sua própria solução de backup do servidor, você pode personalizar essas opções.  
  
-   **Backup local** permite que você execute backups incrementais de nível de bloco regularmente para um disco separado. Como um hoster, você pode anexar um disco rígido virtual para a máquina virtual executando o Windows Server Essentials e, em seguida, configure o backup do servidor para esse disco rígido virtual. O disco rígido virtual deve estar localizado em um disco físico diferente do que a máquina virtual executando o Windows Server Essentials.  
  
    > [!NOTE]
    >  Se você tiver outras soluções de backup de suas máquinas virtuais e não quiser que seus usuários vejam o recurso de Backup do servidor nativo do Windows Server Essentials, você pode desativá-lo e remover a interface do usuário relacionados do painel. Para obter mais informações, consulte o [personalizar o Backup do servidor](https://technet.microsoft.com/library/dn293413.aspx) seção [personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
-   **Backup fora do local** permite que você periodicamente fazer backup de seus dados do servidor para um serviço baseado em nuvem. Você pode baixar e instalar o Microsoft Azure Backup integração módulo para Windows Server Essentials para aproveitar o Backup do Azure que é fornecido pela Microsoft.  
  
     Para obter mais informações, consulte a integrar o Windows Azure Backup com a seção Windows Server Essentials em [gerenciar o Backup do servidor](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md).  
  
     Se você ou seus usuários preferem outro serviço de nuvem, você deve considerar o seguinte:  
  
    -   Atualize a interface do usuário do painel para que ele se vincula ao serviço de nuvem preferencial, em vez de para o padrão de Backup do Azure.  
  
    -   (Opcional) Desenvolva um suplemento para o painel Configurar e gerenciar o serviço de backup baseado em nuvem.  
  
#### <a name="client-computer-backup"></a>Backup do computador cliente  
 Windows Server Essentials Experience dá suporte a dois tipos de backup de dados do cliente: completa de backup do cliente e o histórico de arquivos.  
  
> [!NOTE]
>  Fazer backup do cliente pode afetar o desempenho, pois os dados precisam ser transferidos do cliente para o servidor VPN.  
  
##### <a name="full-client-backup"></a>Backup do cliente completo  
 Backup do cliente completo está ativada por padrão para todos os dispositivos cliente que estão conectados à rede do Windows Server Essentials. Ele totalmente faz backup das informações de sistema e os dados para o cliente e dá suporte a duplicação de dados. Os dados de backup serão armazenados no servidor que executa o Windows Server Essentials. Isso permite que um cliente com falha recuperar dados de um ponto de backup anterior.  
  
 Algumas considerações para backup do computador cliente completo são:  
  
-   **Desempenho** backup do cliente inicial pode ser demorado devido a quantidade de dados a serem carregados.  
  
-   **Estabilidade**, às vezes, a conexão de Internet não é estável no lado do cliente. Backup do cliente é criada para retomar automaticamente e o banco de dados de backup do cliente cria um ponto de controle sempre 40 GB de dados será feito backup. Você pode alterar esse valor para um número menor se você espera que a conexão de Internet não é confiável.  
  
    -   Para habilitar um trabalho checkpoint: no servidor, defina a chave do registro **HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs** como 1.  
  
    -   Para alterar o limite de ponto de verificação: no cliente, alterar **HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold** de seu valor padrão de 40 GB.  
  
-   **Restauração de hardware vazio cliente** porque o ambiente de pré-instalação do Windows não dá suporte a uma conexão de VPN, não há suporte para a restauração bare metal do cliente. Você deve ocultar a tarefa de serviço de restauração de cliente seguindo as etapas em [personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##### <a name="file-history"></a>Histórico de arquivos  
 Histórico de arquivos é um recurso no Windows 8.1 e Windows 8 para fazer backup de dados de perfil (bibliotecas, área de trabalho, contatos, Favoritos) para um compartilhamento de rede. Você pode gerenciar centralmente a configuração de histórico de arquivos de todos os computadores que executam o Windows 8.1 ou Windows 8 que fazem parte de rede do Windows Server Essentials. Os dados de backup são armazenados no servidor que executa o Windows Server Essentials. Você deve ocultar a tarefa de serviço de restauração de cliente seguindo as etapas em [personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
### <a name="storage-management"></a>Gerenciamento de armazenamento  
 Espaços de armazenamento permite que você agregar a capacidade de armazenamento físico dos discos rígidos diferentes, dinamicamente adicione discos rígidos e criar volumes de dados com os níveis especificados de resistência. Você pode fazer isso no host ou na máquina virtual. Se você quiser ocultar esse recurso em uma máquina virtual executando o Windows Server Essentials, siga as instruções em [personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##  <a name="BKMK_Scenarios"></a>Cenários de teste  
 Da perspectiva do host, recomendamos que você teste os seguintes cenários:  
  

-   [Implantação de servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)  
  
-   [Configuração do servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)  
  
-   [Gerenciamento de servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)  
  
-   [Experiência do cliente](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)  

  
###  <a name="BKMK_ServerDeploy"></a>Implantação de servidor  
 Você pode testar os seguintes cenários de implantação do servidor:  
  
-   Implantar um servidor que executa o Windows Server 2012 R2 como um controlador de domínio em seu ambiente de laboratório e, em seguida, instale a função de experiência do Windows Server Essentials.  
  
-   Implantar um servidor que executa o Windows Server 2012 R2 em seu ambiente de laboratório, associar este servidor a um domínio existente e, em seguida, instale a função de experiência do Windows Server Essentials.  
  
-   Personalize a imagem do Windows Server Essentials conforme necessário.  
  
-   Automatize a implantação do Windows Server Essentials com o arquivo autônomo e do Windows PowerShell.  
  
-   Migre servidores de local que executam o Windows Small Business Server para hospedado servidores que executam o Windows Server Essentials.  
  
###  <a name="BKMK_ServerConfig2"></a>Configuração do servidor  
 Você pode testar os seguintes cenários de configuração do servidor:  
  
-   Configure o acesso em qualquer lugar (rede virtual privada, acesso via Web remoto e DirectAccess).  
  
-   Configure pastas de servidor e armazenamento.  
  
-   Ative BranchCache.  
  
-   (Se aplicável) Configure o Backup do servidor, o Backup Online, o Backup do cliente e histórico de arquivos.  
  
-   (Se aplicável) Configurar e gerenciar espaços de armazenamento.  
  
-   (Se aplicável) Configure email integração da solução (Office 365 e servidor do Exchange hospedado).  
  
-   (Se aplicável) Configure a integração com outros serviços online da Microsoft.  
  
-   (Se aplicável) Configure o servidor de mídia.  
  
###  <a name="BKMK_ServerManage"></a>Gerenciamento de servidor  
 Você pode testar os seguintes cenários de gerenciamento de servidor:  
  
-   Gerencie usuários e grupos.  
  
-   Configure e receber notificações de email de alertas.  
  
-   Execute o analisador de práticas recomendadas para ver se há um erro ou uma mensagem de aviso.  
  
-   Configure o pacote do System Center Operations Manager.  
  
-   Configure a recuperação de servidor, em caso de corrupção no sistema operacional.  
  
###  <a name="BKMK_ClientXP"></a>Experiência do cliente  
 Você pode testar os cenários de usuário final a seguir:  
  
-   Implante clientes pela Internet (PC ou Mac sistemas operacionais).  
  
-   Use a barra inicial no cliente acesse pastas compartilhadas.  
  
-   Ativos de servidor de acesso ao longo de acesso via Web remoto de diferentes dispositivos (computador, telefone e tablet).  
  
-   Acesse o aplicativo meu servidor para Windows Phone.  
  
-   (Se aplicável) Histórico de arquivos de acesso, Backup do cliente e restauração e redirecionamento de pasta.  
  
-   (Se aplicável) Verifique se a experiência de integração de email.  
  
##  <a name="BKMK_Support"></a>Informações de suporte  
 Você pode baixar o Windows Server Essentials Software Development Kit (SDK) e a avaliação do Windows Server Essentials e o Kit de implantação (ADK):  
  
-   [Software Development Kit do Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx)SDK  
  
-   [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
## <a name="see-also"></a>Consulte também  
  
-   [Quais são as novidades no Windows Server Essentials](../get-started/what-s-new.md)  

-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)  

-   [Introdução ao Windows Server Essentials](../get-started/get-started.md)
