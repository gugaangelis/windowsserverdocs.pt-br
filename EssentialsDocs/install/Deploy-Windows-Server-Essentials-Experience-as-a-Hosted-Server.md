---
title: Implantar a Experiência do Windows Server Essentials como um servidor hospedado
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: a455c6b4-b29f-4f76-8c6b-1578b6537717
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 5c0e186cb51eadd2671a4a7d21ccc1ce90dd7221
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623533"
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>Implantar a Experiência do Windows Server Essentials como um servidor hospedado

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este documento inclui informações específicas para os hosters que pretendem implantar o Microsoft Windows Server 16 com a função de experiência do Windows Server Essentials (conhecida como Windows Server Essentials no restante do documento) instalada em seu laboratório e pretender oferecer experiência do Windows Server Essentials como um serviço para seus clientes. Este documento inclui as seguintes seções:


-   [Visão geral da Experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)

-   [Benefícios de hospedagem da Experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)

-   [Opções de implantação com suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)

-   [Topologias de rede com suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)

-   [Personalizar a imagem da função da Experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)

-   [Automatizar a implantação da Experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)

-   [Migrar dados do Windows Small Business Server para a Experiência do Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)

-   [Executar tarefas comuns usando o Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)

-   [Integração de email com o Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)

-   [Monitorar e gerenciar usando as ferramentas nativas](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)

-   [Cenários do teste](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)

-   [Informações de suporte](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)


##  <a name="windows-server-essentials-experience-overview"></a><a name="BKMK_WSEEOverview"></a> Visão geral da experiência do Windows Server Essentials
 A experiência do Windows Server Essentials é uma função de servidor que está disponível no Windows Server 2012 R2 Standard e no Windows Server 2012 R2 Datacenter. Quando a função experiência do Windows Server Essentials é instalada em um servidor que executa o Windows Server 2012 R2, o cliente pode aproveitar todos os recursos que estão disponíveis no Windows Server Essentials sem os bloqueios e limites. A experiência do Windows Server Essentials permite as seguintes soluções entre locais para empresas de pequeno e médio porte:

-   **Armazenamento e proteção de dados** Você pode armazenar os dados de "¢ s" do cliente em um local centralizado e proteger os dados do servidor e do cliente, fazendo backup dos computadores cliente e servidor (menos de 75) na rede.

-   **Gerenciamento de usuário** Você pode gerenciar os usuários e grupos por meio do painel de servidor simplificado. Além disso, a integração com o Microsoft Azure Active Directory (AD do Azure) permite o acesso fácil a dados para o Microsoft serviços online (por exemplo, Microsoft 365, Exchange Online e SharePoint Online) para usuários usando suas credenciais de domínio.

-   **Integração de serviço** Você pode integrar o servidor com o Microsoft serviços online (como Microsoft 365, SharePoint Online e Backup do Microsoft Azure). Também é possível integrar o servidor com os serviços ou serviços fornecidos por provedores de terceiros.

-   **Acesso em qualquer lugar** O cliente pode acessar o servidor, os computadores da rede e os dados de praticamente qualquer lugar que tiver uma conexão de Internet e usando quase qualquer dispositivo. O Acesso via Web remoto permite que eles acessem aplicativos e dados com uma experiência de navegador simplificada e compatível com toque. O aplicativo meu servidor permite que eles acessem dados de um Windows Phone ou de um aplicativo Microsoft Store.

-   **Streaming de mídia** Se você instalar o pacote de mídia em um servidor com a experiência do Windows Server Essentials habilitada, o cliente final poderá armazenar música, vídeo e fotografias em pastas compartilhadas e, em seguida, acessar esses arquivos de mídia de computadores em rede ou Acesso via Web remotos.

-   **Monitoramento de integridade** Você pode monitorar a integridade da rede e obter relatórios de integridade personalizados.

##  <a name="benefits-of-hosting-windows-server-essentials-experience"></a><a name="BKMK_Benefits"></a> Benefícios da hospedagem da experiência do Windows Server Essentials
  A experiência do Windows Server Essentials é uma função no Windows Server, portanto, você pode reutilizar a estrutura de implantação e gerenciamento existente no Windows Server para implantar e configurar a função de experiência do Windows Server Essentials. A hospedagem da função de experiência do Windows Server Essentials oferece os seguintes benefícios:

-   **Implantação simplificada** Simplesmente ativando a função de experiência do Windows Server Essentials, algumas das funções e dos recursos mais usados são ativadas e configuradas com as práticas recomendadas para pequenas e médias empresas. Você pode personalizar os recursos do Windows Server Essentials ou ocultar alguns dos recursos locais. Se você usar o Pacote do Microsoft Azure, poderá baixar o modelo de galeria para a experiência do Windows Server Essentials no Windows Server 2012 R2.

-   **Painel simplificado** O Painel do Windows Server Essentials simplifica tarefas comuns, como o gerenciamento de pastas do servidor, armazenamento de servidor, backup e restauração, usuário ou contas de grupo, dispositivos, acesso remoto e email. Clientes de empresas de pequeno e médio porte podem executar as tarefas diárias de gerenciamento em vez de chamar o Suporte para obter suporte técnico.

-   **Extensibilidade** O Painel do Windows Server Essentials e o software Connector do Windows Server Essentials são extensíveis. Você pode adicionar sua própria marca e integração de serviço, para que os clientes tenham um ponto de entrada para tudo sobre seu servidor e o serviço.

-   **Monitor** Uma nova versão do System Center Monitoring Pack está disponível para monitorar e gerenciar vários servidores que executam o Windows Server Essentials. Para baixar o pacote de gerenciamento, consulte [pacote de gerenciamento do System Center para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).

##  <a name="supported-deployment-options"></a><a name="BKMK_SupportedDeployment"></a> Opções de implantação com suporte
  A experiência do Windows Server Essentials pode ser implantada como um controlador de domínio em um novo ambiente de Active Directory; ou pode ser implantado em um ambiente de Active Directory existente como um membro do domínio.

 Recomendamos que você implante primeiro o Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter e, em seguida, instale a função de experiência do Windows Server Essentials. Com esse método de implantação, você obtém todas as funcionalidades do Windows Server Essentials Edition, sem os bloqueios e limites.


 Para obter mais informações sobre como instalar o Windows Server 2012 R2 com a função experiência do Windows Server Essentials, consulte [instalar e configurar o Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).



##  <a name="supported-network-topologies"></a><a name="BKMK_SupportedToplogy"></a> Topologias de rede com suporte
 Para usar a experiência do Windows Server Essentials de um cliente móvel, a VPN deve estar habilitada. Para habilitar o acesso remoto ao servidor de clientes móveis, você precisará abrir a porta 443 e a porta 80 no servidor.

 Aqui estão duas topologias típicas de rede no lado do servidor e como a VPN e o Acesso via Web remoto podem ser configurados:

- **Topologia 1** (Esta é a topologia preferencial e ela coloca todos os servidores e o intervalo de IP da VPN na mesma sub-rede):

  -   Configure o servidor em uma rede virtual separada sob um dispositivo de conversão de endereço de rede (NAT).

  -   Habilite o serviço DHCP na rede virtual ou atribua um endereço IP estático para o servidor.

  -   Encaminhe a porta 443 do IP público no roteador para o endereço de rede local do servidor.

  -   Permita a passagem da VPN para a porta 443.

  -   Defina o conjunto de endereços IPv4 da VPN no mesmo intervalo de sub-rede que o endereço do servidor.

  -   Atribua um segundo servidor ao endereço IP estático na mesma sub-rede, mas fora do conjunto de endereço da VPN.

- **Topologia 2**:

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

##  <a name="customize-the-image-of-windows-server-essentials-experience-role"></a><a name="BKMK_CustomizeImage"></a> Personalizar a imagem da função de experiência do Windows Server Essentials
 Você pode personalizar a imagem antes de configurar a função da Experiência do Windows Server Essentials. Para saber mais sobre o processo padrão do Windows Server Sysprep, consulte [Kit de Avaliação e Implantação do Windows](/previous-versions/windows/hh825420(v=win.10)). Depois de preparar a imagem usando o Sysprep, você pode usá-lo ou selá-lo novamente em Install. wim para uma nova implantação.

 Se estiver usando o Virtual Machine Manager, você pode criar um modelo usando a instância em execução. Esse processo usa o Sysprep para preparar a instância e ele desliga o computador. Depois que você armazená-lo na sua biblioteca, você pode usá-lo caso a caso.

 Depois de instalar a função experiência do Windows Server Essentials, você pode personalizar os recursos no Windows Server Essentials. Uma das personalizações mais importantes é a chave do Registro **IsHosted** : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**.

 Se essa chave estiver definida como 0x1, alguns dos recursos locais terão o comportamento alterado. Essas alterações de recurso incluem:

- **Backup do cliente** O backup de cliente será desativado por padrão para computadores cliente recentemente associados.

- **Serviço de restauração do cliente** O serviço de restauração do cliente será desabilitado e a interface do usuário ficará oculta do Painel.

- **Histórico de arquivos** As configurações do histórico de arquivos para contas de usuário recém-criadas não serão gerenciadas automaticamente pelo servidor.

- **Backup do servidor** O serviço de Backup do servidor será desabilitado e a interface do usuário do servidor de Backup ficará oculta do Painel.

- **Espaços de armazenamento** A interface do usuário para criar ou gerenciar espaços de armazenamento ficará oculta do Painel.

- **Acesso em qualquer local** A configuração do roteador e VPN será ignorada por padrão quando você executar o Assistente de configuração Acesso em qualquer lugar.

  Se você deseja controlar o comportamento de cada recurso listado, você pode definir a chave do Registro correspondente para cada um deles. Para obter informações sobre como definir a chave do Registro, consulte [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-8.1-and-8/dn293241(v=win.10))

##  <a name="automate-the-deployment-of-windows-server-essentials-experience"></a><a name="BKMK_AutomateDeployment"></a> Automatizar a implantação da experiência do Windows Server Essentials
 Para automatizar a implantação, primeiro você precisa implantar o sistema operacional e, em seguida, instalar a função de experiência do Windows Server Essentials.

-   Para implantar automaticamente o Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter, siga as instruções no [Kit de avaliação e implantação do Windows](/previous-versions/windows/hh825420(v=win.10)).

-   Para saber como instalar a função de experiência do Windows Server Essentials usando o Windows PowerShell, consulte [instalar e configurar o Windows Server Essentials](/previous-versions/windows/it-pro/windows-server-essentials-sbs/dn281793(v=ws.11)).

> [!NOTE]
>  Verifique se as configurações de fuso horário da máquina virtual do host e da experiência do Windows Server Essentials são as mesmas. Caso contrário, você pode enfrentar vários erros. Isso inclui: a configuração inicial do servidor pode não ter êxito em tarefas relacionadas ao certificado, o certificado pode não funcionar por algumas horas depois que a função da experiência do Windows Server Essentials for instalada e as informações do dispositivo não serão atualizadas corretamente.

 Após a implantação, use o cmdlet do Windows PowerShell **Get-WssConfigurationStatus** para verificar se a configuração inicial foi bem-sucedida. O status retornado deve ser um dos seguintes: **Notstarted**, **FinishedWithWarning**, **Executando**, **Concluído**, **Falha**ou **PendingReboot**.

 O servidor será reiniciado durante a configuração inicial. Se você precisa impedir esta reinicialização automática, você pode usar o seguinte comando para adicionar uma chave do Registro antes de iniciar a configuração inicial:

```
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false

```

 Depois que a configuração inicial começar, vocé poderá usar **Get-WssConfigurationStatus** para verificar o status da configuração inicial, e, quando o status for **PendingReboot**, poderá reiniciar o servidor.

##  <a name="migrate-data-from-windows-small-business-server-to-windows-server-essentials-experience"></a><a name="BKMK_Migrate"></a> Migre dados do Windows Small Business Server para a experiência do Windows Server Essentials
 Você pode migrar dados de servidores que executam o Windows Small Business Server 2011, o Windows Small Business Server 2008, o Windows Small Business Server 2003 ou o Windows Server Essentials para o servidor que executa o Windows Server Essentials. Examine o guia de migração [migrar para o Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md) para 2migrations locais e faça as personalizações necessárias com base em seu ambiente de hospedagem.

> [!NOTE]
>  Recomenda-se colocar o servidor de origem e o servidor de destino na mesma sub-rede. Se isso não for possível, você deve garantir que:
>
> - O servidor de origem e o servidor de destino podem acessar uns aos outros nomes DNS internos de ¢ s.
>   -   Todas as portas necessárias estejam abertas.

 Após a migração, você pode atualizar suas licenças para remover os bloqueios e limites. Para obter mais informações, consulte [transição do Windows Server Essentials para o Windows server 2012 Standard](/previous-versions/windows/it-pro/windows-server-essentials-sbs/jj247582(v=ws.11)).

##  <a name="perform-common-tasks-by-using-windows-powershell"></a><a name="BKMK_PowerShell"></a> Executar tarefas comuns usando o Windows PowerShell
 Esta seção explica algumas das tarefas comuns que você pode executar usando o Windows PowerShell.

### <a name="enable-remote-web-access"></a>Habilitar Acesso Remoto da Web
 **Sintaxe**:

 Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]

 **Exemplo**:

 $Enable-WssRemoteWebAccess œDenyAccessByDefault œApplyToExistingUsers

 Esse comando permitirá Acesso Remoto da Web com o roteador configurado automaticamente, e trocará as permissões de acesso padrão para todos os usuários existentes.

### <a name="add-user"></a>Adicionar usuário
 **Sintaxe**:

 Add-WssUser [-name] <cadeia \> de caracteres [-password] <SecureString \> [-AccessLevel <String \> {usuário &#124; administrador}] [-FirstName <String \> ] [-LastName <String \> ] [-AllowRemoteAccess] [-AllowVpnAccess] [<CommonParameters \> ]

 **Exemplo**:

 $password = ConvertTo-SecureString "Passw0rd!" -astexto œforce $ Add-WssUser-Name User2Test-password $password-AccessLevel administrador-FirstName Usuário2-LastName Test

 Este comando adicionará um administrador chamado User2Test com a senha Passw0rd!.

### <a name="add-server-folder"></a>Adicionar Pasta do Servidor
 **Sintaxe**:

 Add-WssFolder [-name] <cadeia \> de caracteres [-Path] <cadeia de caracteres \> [[-Description] <cadeia de caracteres \> ] [-KeepPermissions] [<CommonParameters \> ]

 **Exemplo**:

 $Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"

 Esse comando adicionará uma pasta de servidor chamada MyTestFolder no local especificado.

##  <a name="email-integration-with-windows-server-essentials"></a><a name="BKMK_EmailIntegration"></a> Integração de email com o Windows Server Essentials
 Você pode integrar a experiência do Windows Server Essentials com o Microsoft 365 ou o Exchange Server hospedado. Se quiser que o cliente use o email hospedado, precisará desenvolver um complemento para integrar a Experiência do Windows Server Essentials com sua solução de email hospedada. Para obter mais informações, consulte a [SDK do Windows Server Essentials](/previous-versions/windows/server-essentials/gg513877(v=msdn.10)).

##  <a name="monitor-and-manage-by-using-native-tools"></a><a name="BKMK_Monitoring"></a> Monitorar e gerenciar usando ferramentas nativas
 Esta seção aborda as ferramentas nativas que estão disponíveis no Windows Server 2012 R2 para monitorar e gerenciar o servidor.

### <a name="group-policy"></a>Política de Grupo
  A experiência do Windows Server Essentials aproveita o suporte nativo do Política de Grupo no Windows Server 2012 R2 e fornece uma interface do usuário para configurar o redirecionamento de pasta e as configurações de segurança.

> [!NOTE]
>  Em um ambiente hospedado, se o Redirecionamento de pasta para um perfil de usuário estiver habilitado, ele pode aumentar o tempo para o usuário final entrar quando o tamanho dos dados for grande.

### <a name="system-center-monitoring-pack"></a>Pacote de monitoramento do System Center
 A experiência do pacote de monitoramento do System Center para Windows Server Essentials monitora o sistema de alertas de integridade para ajudá-lo a gerenciar grandes números de servidores que executam o Windows Server Essentials, que são dedicados a organizações de pequenas empresas. Para obter mais informações, consulte [pacote de gerenciamento do System Center para Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).

### <a name="backup-and-restore"></a>Backup e restauração
  O Windows Server 2012 R2 com a experiência do Windows Server Essentials permite fazer backup de computadores cliente e servidor na rede.

#### <a name="server-backup"></a>Backup do servidor
 O Windows Server Essentials oferece suporte a duas maneiras de fazer backup do servidor: backup local e backup externo. Se você deseja implantar sua própria solução de backup do servidor, você pode personalizar essas opções.

-   O**backup local** permite que você realize backup incremental no nível do bloco regularmente em um disco separado. Como hoster, você pode anexar um disco rígido virtual à máquina virtual que executa o Windows Server Essentials e, em seguida, configurar o backup do servidor nesse disco rígido virtual. O disco rígido virtual deve estar localizado em um disco físico diferente da máquina virtual que executa o Windows Server Essentials.

    > [!NOTE]
    >  Se você tiver outras soluções de backup para as máquinas virtuais e não quiser que os usuários vejam o recurso de Backup do servidor nativo do Windows Server Essentials, você pode desativá-lo e remover a interface do usuário relacionada do Painel. Para obter mais informações, consulte a seção [Personalizar backup do servidor](/previous-versions/windows/it-pro/windows-8.1-and-8/dn293413(v=win.10)) de [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-8.1-and-8/dn293241(v=win.10)).

-   **Backup externo** Permite periodicamente fazer backup de dados do servidor para um serviço baseado em nuvem. Você pode baixar e instalar o módulo integração do Backup do Microsoft Azure para o Windows Server Essentials para aproveitar o backup do Azure fornecido pela Microsoft.

     Para obter mais informações, consulte a seção integrar o backup do Windows Azure com o Windows Server Essentials em [gerenciar o backup do servidor](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md).

     Se você ou seus usuários preferirem outro serviço de nuvem, você deve:

    -   Atualize a interface do usuário do painel para que ele se vincule ao serviço de nuvem preferido, e não ao backup padrão do Azure.

    -   (Opcional) Desenvolva um complemento para o Painel para configurar e gerenciar o serviço de backup baseado em nuvem.

#### <a name="client-computer-backup"></a>Backup do Computador Cliente
 O Experiência do Windows Server Essentials oferece suporte a dois tipos de backup de dados do cliente: backup do cliente completo e histórico de arquivos.

> [!NOTE]
>  Realizar o backup do cliente pode ter impacto sobre o desempenho, pois os dados precisam ser transferidos do cliente para o servidor por VPN.

##### <a name="full-client-backup"></a>Backup completo do cliente
 O backup completo do cliente é ativado por padrão para todos os dispositivos de cliente conectados à rede Windows Server Essentials. Ele faz backup completo das informações de sistema e dados para o cliente e oferece suporte à eliminação de duplicação de dados. Os dados de backup serão armazenados no servidor que executa o Windows Server Essentials. Isso permite que um cliente com falha recupere dados de um ponto de backup anterior.

 Algumas considerações para o backup de computador completo do cliente são:

-   **Desempenho** O backup inicial do cliente pode ser demorado em função da quantidade de dados a serem carregados.

-   **Estabilidade** Às vezes, a conexão com a Internet não é estável no lado do cliente. O backup do cliente é projetado para ser retomado automaticamente e o banco de dados de backup do cliente cria um ponto de verificação toda vez que é feito o backup de 40 GB de data. Você pode alterar esse valor para um número menor se esperar que a conexão com a Internet seja pouco confiável.

    -   Para habilitar uma tarefa de ponto de verificação: No servidor, defina a chave do Registro **HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs** como 1.

    -   Para alterar o limite do ponto de verificação: No cliente, altere **HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold** do seu valor padrão de 40 GB.

-   **Restauração Bare Metal do Cliente** Como o Ambiente de Pré-instalação do Windows não tem suporte para conexão VPN, não há suporte para a Restauração Bare Metal do Cliente. Você deve ocultar a tarefa de serviço de restauração do cliente seguindo as etapas em [Personalizar e implantar o Windows Server Essentials no Windows server 2012 R2](/previous-versions/windows/it-pro/windows-8.1-and-8/dn293241(v=win.10)).

##### <a name="file-history"></a>Histórico de arquivos
 Histórico de arquivos é um recurso no Windows 8.1 e Windows 8 para fazer backup dos dados de perfil (bibliotecas, área de trabalho, contatos, favoritos) para um compartilhamento de rede. Você pode gerenciar centralmente a configuração do Histórico de arquivos de todos os computadores que executam o Windows 8.1 ou Windows 8 que fazem parte da rede do Windows Server Essentials. Os dados de backup são armazenados no servidor executando o Windows Server Essentials. Você deve ocultar a tarefa de Serviço de restauração do cliente seguindo as etapas em [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-8.1-and-8/dn293241(v=win.10))

### <a name="storage-management"></a>Gerenciamento de armazenamento
 Os espaços de armazenamento permitem agregar a capacidade de armazenamento físico de discos rígidos diferentes, incluir dinamicamente discos rígidos e criar volumes de dados com níveis especificados de resiliência. Você pode fazer isso no host ou na máquina virtual. Se você quiser ocultar esse recurso em uma máquina virtual que executa o Windows Server Essentials, siga as instruções em [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-8.1-and-8/dn293241(v=win.10)).

##  <a name="test-scenarios"></a><a name="BKMK_Scenarios"></a> Cenários de teste
 Da perspectiva de hospedagem, recomendamos que você teste os seguintes cenários:


-   [Implantação de Servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)

-   [Configuração do servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)

-   [Gerenciamento de servidor](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)

-   [Experiência do cliente](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)


###  <a name="server-deployment"></a><a name="BKMK_ServerDeploy"></a> Implantação de servidor
 Você pode testar os seguintes cenários de implantação do servidor:

-   Implante um servidor que executa o Windows Server 2012 R2 como um controlador de domínio em seu ambiente de laboratório e, em seguida, instale a função de experiência do Windows Server Essentials.

-   Implante um servidor que executa o Windows Server 2012 R2 em seu ambiente de laboratório, ingresse esse servidor em um domínio existente e, em seguida, instale a função de experiência do Windows Server Essentials.

-   Personalize a imagem do Windows Server Essentials conforme necessário.

-   Automatize a implantação do Windows Server Essentials com arquivo autônomo e Windows PowerShell.

-   Migre servidores locais que executam o Windows Small Business Server para servidores hospedados que executam o Windows Server Essentials.

###  <a name="server-configuration"></a><a name="BKMK_ServerConfig2"></a> Configuração do servidor
 Você pode testar os seguintes cenários de configuração do servidor:

-   Configure o Acesso em qualquer lugar (rede privada virtual, Acesso via Web remoto e DirectAccess).

-   Configurar o Armazenamento e as Pastas do Servidor.

-   Ative o BranchCache.

-   (Se aplicável) Configure Backup de Servidor, Backup Online, Backup do Cliente, Histórico de Arquivos.

-   (Se aplicável) Configurar e gerenciar os Espaços de Armazenamento.

-   (Se aplicável) Configurar a integração de solução de email (Microsoft 365 e Exchange Server hospedado).

-   (Se aplicável) Configure a integração com outros serviços online da Microsoft.

-   (Se aplicável) Configurar o Servidor de Mídia.

###  <a name="server-management"></a><a name="BKMK_ServerManage"></a> Gerenciamento de servidor
 Você pode testar os seguintes cenários de gerenciamento de servidor:

-   Gerencie usuários e grupos.

-   Configurar e receber notificações de alertas por email.

-   Execute o Analisador de práticas recomendadas para ver se há uma mensagem de erro ou aviso.

-   Configure o pacote do System Center Operations Manager.

-   Configure a recuperação do servidor, em caso de corrupção no sistema operacional.

###  <a name="client-experience"></a><a name="BKMK_ClientXP"></a> Experiência do cliente
 Você pode testar os seguintes cenários de usuário final:

-   Implante clientes pela Internet (PC ou sistemas operacionais Mac).

-   Usa a Barra Inicial no cliente para acessar Pastas Compartilhadas.

-   Acesse ativos do servidor por Acesso Remoto da Web de diferentes dispositivos (PC, telefone, tablet).

-   Acesse o aplicativo My Server para Windows Phone.

-   (Se aplicável) Acesse o Histórico de arquivos, Backup e restauração do cliente Redirecionamento de pasta.

-   (Se aplicável) Verifique a experiência de integração de email.

##  <a name="support-information"></a><a name="BKMK_Support"></a> Informações de suporte
 Você pode baixar o SDK (Software Development Kit) do Windows Server Essentials e o ADK (Kit de avaliação e implantação) do Windows Server Essentials:

-   [Software Development Kit do Windows Server Essentials](/previous-versions/windows/server-essentials/gg513877(v=msdn.10)) SDK

-   [Personalizar e implantar o Windows Server Essentials no Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-8.1-and-8/dn293241(v=win.10))

## <a name="additional-references"></a>Referências adicionais

-   [O que há de novo no Windows Server Essentials](../get-started/what-s-new.md)

-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)

-   [Introdução ao Windows Server Essentials](../get-started/get-started.md)