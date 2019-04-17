---
title: Hospedado do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fda5628c-ad23-49de-8d94-430a4f253802
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 23e586c22ca14af7b02550e2fa1c9522e379170c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="hosted-windows-server-essentials"></a>Hospedado do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este documento inclui informações que são específicas para hosters que pretendem implantar o Windows Server Essentials no seu laboratório e oferecer o Windows Server Essentials como um serviço para seus clientes.  
  
## <a name="what-is-windows-server-essentials"></a>Qual é o Windows Server Essentials?  
 Windows Server Essentials é uma solução de pequenas empresas entre locais, que incorpora tecnologias de produto de ponta de 64 bits para proporcionar um ambiente de servidor adequado para a maioria das pequenas empresas. As seguintes tecnologias estão incluídas no Windows Server Essentials.  
  
 **Sistema operacional de servidor:** tecnologias de produto do Windows Server 2012 fornecem o núcleo do Windows Server Essentials. Para obter mais informações, visite o [site do Windows Server 2012](https://www.microsoft.com/en-us/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh).  
  
 **Proteção de dados:** Windows Server Essentials utiliza vários novos recursos disponíveis no Windows Server 2012 para fornecer recursos de proteção de dados aprimorada. O [novo recurso de espaços de armazenamento](https://technet.microsoft.com/library/hh831739.aspx) permite que você agregue a capacidade de armazenamento físico dos discos rígidos diferentes, dinamicamente adicionar discos rígidos e criar volumes de dados com os níveis especificados de resistência. Windows Server Essentials pode executar backups do sistema completo e bare-metal restaura do servidor em si, bem como os computadores cliente conectados à rede? agora com suporte para volumes maiores do que 2 TB. Novidades do Windows Server 2012, o [Windows Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx) podem ser usados para proteger arquivos e pastas em um serviço de armazenamento baseado em nuvem que é gerenciada pela Microsoft. Windows Server Essentials também centralmente gerencia e configura o recurso de histórico de arquivos de clientes do Windows 8.1, ajudando os usuários recuperarem de arquivos acidentalmente excluídos ou substituídos sem a necessidade de assistência de administrador.  
  
 **Em qualquer lugar acesso:** acesso via Web remoto oferece uma experiência de navegador simplificada e sensível ao toque para acessar aplicativos e dados de praticamente qualquer lugar, você tem uma conexão de Internet e usando praticamente qualquer dispositivo. Windows Server Essentials também fornece um aplicativo do Windows Phone atualizado e um novo aplicativo cliente do Windows 8.1 de computadores, permitindo que os usuários intuitivamente se conectar, pesquise e acessar arquivos e pastas no servidor. Os arquivos também automaticamente são armazenadas em cache para acesso offline e sincronizados quando uma conexão para o servidor estiver disponível. Windows Server Essentials desativa a configuração de rede virtual privada (VPN) em um processo simples orientadas pelo Assistente de apenas alguns cliques e simplifica o gerenciamento de acesso VPN para os usuários. Computadores cliente podem aproveitar uma conexão VPN para participar remotamente o ambiente do Windows SBS sem a necessidade de caminho para o escritório.  
  
 **Flexibilidade de carga de trabalho:** Windows Server Essentials foi projetado para permitir que os clientes a flexibilidade escolher quais aplicativos e serviços executados no local e que executados na nuvem. Em versões anteriores, o Windows Small Business Server Standard incluído servidor do Exchange como um produto de componente, que adicionou despesa e da complexidade para clientes que queria aproveitar os serviços de mensagens e colaboração baseada na nuvem. Com o Windows Server Essentials, os clientes podem tirar proveito do mesmo tipo de experiência de gerenciamento integrado se ele optarem por executar uma cópia local do servidor do Exchange, assinar um serviço hospedado do Exchange ou inscrever-se para o Microsoft Office 365.  
  
 **Monitoramento de integridade:** Windows Server Essentials monitora seu próprio status de integridade e o status dos computadores cliente que executam o Windows 8.1, Windows 7 e Mac OS X versão 10,5 e versões posteriores. Status de integridade notifica você sobre problemas ou problemas relacionados a backups do computador, armazenamento do servidor, pouco espaço em disco e muito mais.  
  
 **Extensibilidade:** compilações do Windows Server Essentials no modelo de extensibilidade do Windows SBS 2011 Essentials, que permite que outros fornecedores de software adicionar recursos e funcionalidades ao produto principal e adiciona um novo conjunto de web APIs de serviços. Ele também mantém a compatibilidade com existente [software development kit do](https://msdn.microsoft.com/library/gg513958.aspx) (SDK) e [suplementos](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials) criado para o Windows SBS 2011 Essentials.  
  
## <a name="how-can-i-customize-an-image"></a>Como posso personalizar uma imagem?  
 Consulte o [Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124), que é um processo de sysprep padrão do Windows Server com etapas adicionais de personalização do Windows Server Essentials. Para concluir a personalização, siga as instruções em [criar uma imagem personalizada simples](https://technet.microsoft.com/en-us/library/jj200117) e [personalizar a imagem](https://technet.microsoft.com/en-us/library/jj200161)e siga as instruções em [Preparando a imagem para implantação](https://technet.microsoft.com/en-us/library/jj200142) para capturar a imagem final.  
  
 Você deve prestar atenção para os seguintes pontos:  
  
1.  Você deve ignorar a configuração inicial (IC), adicionando um arquivo SkipIC.txt para a raiz de qualquer unidade. Depois de instalar o servidor, antes de IC, pressione Shift + F10 para iniciar a janela cmd e crie um arquivo SkipIC.txt na unidade c: / drive. Após a personalização, lembre-se excluir o arquivo SkipIC.txt.  
  
2.  Se você precisar implantar o Windows Server Essentials em um disco menor que 90 GB, você deve adicionar uma chave do registro antes do sysprep:  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
 Depois de sysprep, você pode usar a imagem de disco do Sysprep ou reseal-lo de volta em Install.wim para nova implantação.  
  
 Se você estiver usando o Gerenciador de máquina Virtual, você pode criar um modelo usando a instância em execução. Criando um modelo será a instância de sysprep e desligar o servidor. Depois que você pode armazená-lo em sua biblioteca, você pode ativar a instância caso a caso.  
  
##  <a name="BKMK_automatedeployment"></a>Como automatizar a implantação?  
 Depois de obter a imagem personalizada, você pode fazer a implantação com sua própria imagem. Para fazer a instalação semiunattended, você precisa fornecer/implantar unattend.xml para a instalação do WinPE. Para fazer uma instalação totalmente autônoma, você também precisa fornecer o arquivo cfg.ini para configuração inicial do Windows Server Essentials.  
  
1.  Execute apenas as instalação autônoma do WinPE. Isso será automatizar somente a instalação do WinPE e permitir que a instalação parar antes da configuração inicial para que os usuários finais possam fornecer informações Corp, domínio e administrador por si só após RDP na sessão do servidor. Para fazer isso:  
  
    1.  Forneça o arquivo unattend.xml do Windows. Siga o [ADK do Windows 8.1](https://go.microsoft.com/fwlink/?LinkId=248694) para gerar o arquivo e fornecer todas as informações necessárias, incluindo o nome do servidor, chaves do produto e senha de administrador. Na seção Microsoft-Windows-Setup do arquivo unattend.xml, forneça as informações abaixo.  
  
        ```  
        <InstallFrom>  
                 <MetaData>  
                     <Key>IMAGE/WINDOWS/EDITIONID</Key>  
                     <Value>ServerSolution</Value>  
                 </MetaData>  
                 <MetaData>  
                     <Key>IMAGE/WINDOWS/INSTALLATIONTYPE</Key>  
                     <Value>Server</Value>  
                 </MetaData>  
           </InstallFrom>  
        ```  
  
    2.  Porta RDP 3389 deve ser aberta em um IP público para que o cliente pode usar o administrador e a senha especificada no arquivo unattend.xml para RDP ao servidor para concluir a configuração inicial.  
  
    > [!NOTE]
    >  Se você não alterar a senha padrão, a instalação do servidor interromperá em uma tela perguntando para uma senha para ser inserido. **Observação** os usuários finais devem usar a conta de administrador padrão para fazer logon no servidor e realizar a configuração inicial.  
  
 Se você estiver usando o Gerenciador de máquina Virtual, você pode especificar a senha de administrador no console quando você cria uma nova instância do modelo.  
  
1.  Execute concluir a instalação autônoma incluindo autônoma configuração inicial. Para fazer isso:  
  
    1.  Forneça o arquivo unattend.xml como fez acima, se começar a implantação de instalação do WinPE.  
  
    2.  Consulte a seção Windows Server Essentials ADK qualificada, [criá-lo Cfg.ini](https://technet.microsoft.com/en-us/library/jj200150), para gerar o cfg.ini.  
  
    3.  Fornece informações em [InitialConfiguration].  
  
        ```  
        WebDomainName=yourdomainname  
        TrustedCertFileName=c:\cert\a.pfx  
        TrustedCertPassword=Enteryourpassword  
        EnableVPN=true  
        EnableRWA=true  
        ; Provide all information so that after setup is complete, your customer can use your domain name to visit the server directly with the admin/user information you provide in the [InitialConfiguration] section.  
  
        VpnIPv4StartAddress=<IPV4Address>  
        VpnIPv4EndAddress=<IPV4Address>  
        VpnBaseIPv6Address=<IPV6Address>  
        VpnIPv6PrefixLength=<number>  
        ; Provide this information. IPv4StartAddress and IPv4Endaddress are required so that your VPN client can acquire valid IP through this range.  
  
        IPv4DNSForwarder=<IPV4Address,IPV4Address,Â¦>  
        IPv6DNSForwarder=<IPV6Address,IPV6Address,Â¦>  
        ; Provide this information as needed according to your network environment settings.  
        ```  
  
    4.  Fornece informações em [PostOSInstall].  
  
        ```  
        IsHosted=true   
        ; Must have, this will prevent Initial Configure webpage available for other computers under same subnet.  
  
        StaticIPv4Address=<IPV4Address>  
        StaticIPv4Gateway=<IPV4Address>  
        StaticIPv6Address=<IPV6Address>  
        StaticIPv6SubnetPrefixLength=<number>  
        StaticIPv6Gateway=<IPV6Address>  
        ; All these are optional if you have DHCP Server Service on the subnet, otherwise provide static IP here.  
        ```  
  
    5.  Se você fornecer o parâmetro WebDomainName, verifique se o registro DNS também estiver sendo atualizado para apontar para o IP do servidor s pública.  
  
    6.  Se você não forneceu as informações de WebDomainName acima, verifique se abrir porta 3389 para que os clientes possam usar RDP para se conectar ao servidor e concluir a configurações de VPN.  
  
> [!NOTE]
>  Certifique-se de que a configuração de fuso horário do servidor host VM e a VM do Windows Server Essentials é o mesmo. Caso contrário, você pode enfrentar vários erros diferentes (configuração inicial pode falhar em tarefas relacionadas ao certificado; certificado talvez não funcionem até algumas horas após a instalação, as informações do dispositivo não serão atualizados corretamente; e assim por diante).  
  
 Após a implantação, verifique a seguinte chave do registro em HKLM\software\microsoft\windows server\setup para verificar se a configuração inicial foi bem-sucedida. Se SetupStage = = ICDone & & ICStatus = = 1, isso significa que a configuração inicial foi concluída com êxito.  
  
## <a name="what-is-the-supported-network-topology"></a>Qual é a topologia de rede com suporte?  
 Para usar o Windows Server Essentials de um cliente móvel, VPN deve ser habilitada. É recomendável ativar porta 443 para conexões VPN SSTP. Se o recurso de servidor de mídia é necessária para aplicativos de acesso via Web remoto ou serviço da Web, porta 80 também deve ser habilitada.  
  
 Habilitação de VPN pode ser feito durante a implantação autônoma por meio de nosso script do Windows PowerShell, ou pode ser configurado com nosso Assistente após a configuração inicial.  
  

-   Para habilitar VPN durante a implantação autônoma, consulte [como automatizar a implantação? ](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) neste documento.  

-   Para habilitar VPN durante a implantação autônoma, consulte [como automatizar a implantação? ](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) neste documento.  

  
-   Para habilitar a VPN por meio do Windows PowerShell, execute o seguinte cmdlet com privilégios administrativos e fornecer todas as informações necessárias.  
  
    ```  
    ##  
    ## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
    ##  
  
    $myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
    $mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
    $mySslCertificatePassword = ConvertTo-SecureString '******';   ## password for private key of the SSL certificate  
    $skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
    Add-Type -AssemblyName 'Wssg.Web.DomainManagerObjectModel';  
    [Microsoft.WindowsServerSolutions.RemoteAccess.Domains.DomainConfigurationHelper]::SetDomainNameAndCertificate($myExternalDomainName,$mySslCertificateFile,$mySslCertificatePassword,$skipCertificateVerification);  
    ##  
    ## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
    ##  
  
    Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
    ```  
  
 Se você não pode fornecer uma conexão de VPN antes de conceder o servidor para os clientes, certifique-se de que a porta do servidor 3389 está acessível na Internet para que os usuários finais podem usar o RDP para se conectar ao servidor e fazer a configuração por si só.  
  
 Aqui estão as topologias de redes de servidor típicas dois e como a VPN/controle remoto da Web Access (RWA) podem ser configuradas:  
  
-   Topologia 1 (preferida)  
  
    -   Servidor em uma rede virtual separada em um dispositivo NAT.  
  
    -   Serviço DHCP está habilitado na rede virtual ou o servidor é atribuído com um endereço IP estático.  
  
    -   Porta do servidor 443 está acessível da porta IP pública 443.  
  
    -   Passagem VPN é permitida para porta 443.  
  
    -   Pool de endereço IPv4 VPN deve ser faixa na mesma sub-rede do endereço do servidor.  
  
    -   Qualquer servidor da segunda deve ser atribuído um IP estático dentro da mesma sub-rede, mas sem o pool de endereços VPN.  
  
-   Topologia 2:  
  
    -   O servidor tem um endereço IP privado.  
  
    -   Porta 443 no servidor é acessível de uma público endereço IP s porta 443.  
  
    -   Passagem VPN é permitida para porta 443.  
  
    -   Pool de endereço IPv4 VPN está em um intervalo diferente do endereço de servidor.  
  
 Com topologia 2, não há suporte para cenários de servidor segundos.  
  
## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>Como realizar tarefas comuns por meio do Windows PowerShell?  
 **Habilitar o acesso via Web remoto**  
  
```  
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
```  
  
 Exemplo:  
  
```  
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
```  
  
 Esse comando habilitar o acesso via Web remoto com o roteador configurado automaticamente e alterar as permissões de acesso padrão para todos os usuários existentes.  
  
 **Adicionar usuário**  
  
```  
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]  
```  
  
 Exemplo:  
  
```  
$password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce  
$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
```  
  
 Esse comando adicionará um administrador denominado User2Test com senha Passw0rd!.  
  
 **Habilitar/desabilitar usuário**  
  
 Exemplo:  
  
```  
$CurrentUser = get-wssuser  œname user2test  
$CurrentUser.UserStatus = 0  
$CurrentUser.Commit()  
```  
  
 Esse comando desabilita o usuário identificado user2test. Configuração UserStatus como 1 permitirá que o usuário.  
  
 **Adicionar pastas de servidor**  
  
```  
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]  
```  
  
 Exemplo:  
  
```  
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
```  
  
 Esse comando irá adicionar uma pasta de servidor chamada MyTestFolder no local especificado.  
  
## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>Como adicionar um segundo servidor no domínio do Windows Server Essentials?  
 Como o Windows Server Essentials é um controlador de domínio, você pode associar um segundo servidor no domínio do modo padrão.  
  
## <a name="which-email-solutions-can-be-integrated"></a>As soluções de email podem ser integradas?  
 Windows Server Essentials oferece suporte à integração com duas soluções de email prontamente: Office 365 e Exchange local. Se você estiver executando sua própria solução de email hospedadas, você precisará desenvolver um suplemento para integrar o Windows Server Essentials com sua solução email hospedado.  
  
## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>Como migrar no local Windows SBS (2008/2011/2003) para o Windows Server Essentials hospedado?  
 Guias de migração estão disponíveis para o local Windows Small Business Server (Windows SBS) para migrações do Windows Server Essentials. Algumas das etapas não podem aplicar exatamente o mesmo para o seu ambiente hospedado. No entanto, as tarefas gerais e as cargas de trabalho devem ser migrados devem ser os mesmos. Recomendamos que você consulte o [guias de migração](https://go.microsoft.com/fwlink/p/?LinkID=254292) e faça as personalizações necessárias com base em seu ambiente de hospedagem.  
  
 É recomendável que você colocar o servidor de origem e o servidor de destino na mesma sub-rede. Se isso não for possível, você deve garantir que:  
  
-   O nome DNS interno do servidor de origem e destino são acessíveis por uns aos outros.  
  
-   Todas as portas necessárias estão abertas.  
  
## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>Como posso atualizar o Windows Server Essentials para o Windows Server Standard?  
 Você pode atualizar o Windows Server Essentials para Windows Server Standard. Remover bloqueios e limites e adicione os pacotes que estão faltando no Windows Server Standard. Para obter mais informações, [baixar o documento](https://go.microsoft.com/fwlink/p/?LinkID=253181).  
  
## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>Quais são as ferramentas de monitoramento e gerenciamento nativas?  
  
### <a name="group-policy-management"></a>Gerenciamento de política de grupo  
 Windows Server Essentials aproveita o suporte nativo de política de grupo no Windows Server 2012 e fornece a interface do usuário para definir as configurações de segurança e redirecionamento de pasta.  
  
> [!NOTE]
>  Em um ambiente hospedado, se o redirecionamento de pasta para um perfil de usuário estiver habilitado, ele pode aumentar o tempo para os usuários finais façam logon quando o tamanho dos dados é grande.  
  
### <a name="management-pack"></a>Pacote de gerenciamento  
 Pacote de gerenciamento do Windows Server Essentials oferece monitoramento função sobre o sistema de alerta de integridade no Windows Server Essentials para ajudar hosters gerenciar um grande número de servidores do Windows Server Essentials dedicados para empresas diferentes pequenas empresas. O monitoramento nesta versão inclui apenas os alertas essenciais do sistema.  
  
#### <a name="management-pack-scope"></a>Escopo do pacote de gerenciamento  
 Este pacote de gerenciamento ajuda você a monitorar os recursos específicos para o Windows Server Essentials. Ele não monitora recursos que são genéricos no sistema operacional Windows Server 2012 Standard. Para monitorar o Windows Server Essentials, você deve usar o pacote de gerenciamento do Windows Server Essentials e o gerenciamento de pacote para o Windows Server 2012 Standard.  
  
#### <a name="mandatory-configuration"></a>Configuração obrigatória  
 As etapas a seguir precisam ser tomada antes que você pode usar o pacote de gerenciamento:  
  
1.  Instale o agente e configure a relação de confiança usando certificados confiáveis. Como o Windows Server Essentials é pré-configurado como um controlador de domínio e não pode ter confiança com outros domínios ou florestas, o agente de Gerenciador de operação do Centro de sistema devem ser instalado no Windows Server Essentials e configurado de confiança com o uso de certificados do servidor de gerenciamento.  
  
2.  Baixe o pacote de gerenciamento. Para monitorar o Windows Server Essentials usando Operations Manager 2007, você deve primeiro baixar o [pacote de gerenciamento de sistema operacional Windows Server](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010) do catálogo de pacote de gerenciamento.  
  
3.  Importe o arquivo do pacote de gerenciamento. Se você estiver usando uma versão localizada do management pack, você precisa importar o arquivo do pacote de gerenciamento principal e o pacote de idiomas.  
  
#### <a name="files-in-this-monitoring-pack"></a>Arquivos neste pacote de monitoramento  
 O monitoramento Pack para Windows Server Essentials inclui os seguintes arquivos:  
  
-   Microsoft.Windows.Server.2012.Essentials.mp  
  
-   Microsoft.Windows.Server.2012.Essentials. < locale\ >. MP  
  
### <a name="back-up-and-restore"></a>Backup e restauração  
 Windows Server Essentials permite que você faça backup do servidor e cliente.  
  
#### <a name="back-up-the-server"></a>Fazer backup do servidor  
 Windows Server Essentials oferece duas maneiras de fazer backup do servidor: backup de local e backup fora do local.  
  
 **Backup local** permite realizar o backup incremental do nível de bloco regularmente para um disco separado. Como um hoster, você pode anexar um disco virtual para a VM do Windows Server Essentials e configure o backup do servidor para esse disco virtual. O disco virtual deve estar localizado em um disco físico diferente do que a VM do Windows Server Essentials.  
  
-   Se você tiver outro mecanismo para fazer backup a VM do Windows Server Essentials, e você não quiser que seus usuários para ver o recurso de Backup do servidor Windows Server Essentials nativo, você pode desativá-lo e remover todas as interface do usuário relacionados no Windows Server Essentials painel. Para obter mais informações, consulte a seção personalizar o Backup do servidor do [documento ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
 **Backup fora do local** permite que você periodicamente fazer backup dos dados do servidor para um serviço de nuvem. Você pode baixar e instalar o Microsoft Azure Backup integração módulo para Windows Server Essentials para aproveitar o Backup do Azure fornecido pela Microsoft.  
  
 Se você ou seus usuários preferem outro serviço de nuvem, você deve:  
  
1.  Atualize a interface do usuário do Windows Server Essentials painel para que ele fornece um link para seu serviço de nuvem preferencial, em vez do padrão de Backup do Azure. Para obter mais informações, consulte Personalizar a seção de imagem da [documento ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
2.  (Opcional) Desenvolva um suplemento para o Windows Server Essentials Dashboard configurar e gerenciar o serviço de backup na nuvem.  
  
#### <a name="back-up-the-client"></a>Faça backup do cliente  
 Windows Server Essentials dá suporte a dois tipos de backup de dados do cliente: backup do cliente completo e histórico de arquivos.  
  
> [!NOTE]
>  Fazer backup do cliente pode afetar o desempenho, pois os dados precisam ser transferidos do cliente para o servidor VPN.  
  
 **Backup do cliente completo** destina-se por padrão em todos os dispositivos cliente conectados à rede Windows Server Essentials. Ele faz o backup do cliente completo (sistema e dados) incrementalmente e dá suporte a duplicação de dados. Os dados de backup será no servidor que executa o Windows Server Essentials. Um cliente com falha pode obter seus dados de volta para um ponto de backup anterior. Você pode desativar esse recurso seguindo as etapas em Criar seção arquivo Cfg.ini o [documento ADK](https://technet.microsoft.com/en-us/library/jj200150).  
  
 Algumas considerações para backup completo do cliente são:  
  
-   Desempenho: backup do cliente inicial pode ser demorado devido a quantidade de dados a serem carregados.  
  
-   Estabilidade: às vezes, a conexão de Internet não é estável no lado do cliente. Backup do cliente foi projetado para ser reiniciáveis e o ponto de controle padrão é 40 GB (o banco de dados de backup do cliente criará um ponto de controle sempre que dados de 40 GB foi feitos o backup). Você pode alterar esse valor para um número menor se você espera que a conexão de Internet não é confiável.  
  
    -   Para habilitar um trabalho de ponto de controle, no servidor, defina a chave do registro HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs como 1.  
  
    -   Para alterar o limite de ponto de controle, no cliente, altere HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold de seu valor padrão (40 GB).  
  
-   Restauração de hardware vazio de cliente: como ambiente de pré-instalação do Windows não dá suporte a conexão de VPN, a restauração do cliente Bare Metal não é permitido.  
  
 **Histórico de arquivos** é um recurso do Windows 8.1 para fazer backup de dados de perfil (bibliotecas, área de trabalho, contatos, Favoritos) para um compartilhamento de rede. No Windows Server Essentials, permitimos que gerenciamento central da configuração histórico de arquivos de todos os clientes do Windows 8.1 associado ao Windows Server Essentials. Os dados de backup são armazenados no servidor que executa o Windows Server Essentials. Você pode desativar esse recurso seguindo as etapas em Criar seção arquivo Cfg.ini o [documento ADK](https://technet.microsoft.com/en-us/library/jj200150).  
  
### <a name="storage-management"></a>Gerenciamento de armazenamento  
 O [novo recurso de espaços de armazenamento](https://technet.microsoft.com/library/hh831739.aspx) permite que você agregue a capacidade de armazenamento físico dos discos rígidos diferentes, dinamicamente adicionar discos rígidos e criar volumes de dados com os níveis especificados de resistência. Você também pode anexar um disco iSCSI ao Windows Server Essentials para expandir seu armazenamento.  
  
## <a name="what-are-the-main-scenarios-i-should-test"></a>Quais são os cenários principais que deve testar?  
 Da perspectiva do host, é recomendável que você teste os seguintes cenários:  
  
 **Implantação de servidor**  
  
-   Implante o servidor Windows Server Essentials em seu ambiente de laboratório.  
  
-   Personalize a imagem do Windows Server Essentials conforme necessário.  
  
-   Automatize a implantação do Windows Server Essentials com o arquivo autônomo e cfg.ini.  
  
-   Migre no local Windows SBS para o Windows Server Essentials hospedado.  
  
-   Atualização do Windows Server Essentials para o Windows Server 2012.  
  
 **Configuração do servidor**  
  
-   Em qualquer lugar configure o acesso (DirectAccess VPN, acesso via Web remoto,).  
  
-   Configure o armazenamento e a pasta no servidor.  
  
-   (Se aplicável) Configure o Backup do servidor, o Backup Online, o Backup do cliente, histórico de arquivos.  
  
-   (Se aplicável) Configurar e gerenciar espaços de armazenamento.  
  
-   (Se aplicável) Configure email integração da solução (Office 365, hospedado Exchange e assim por diante).  
  
-   (Se aplicável) Configure o servidor de mídia.  
  
 **Gerenciamento de servidor**  
  
-   Gerencie usuários.  
  
-   Configure e receber notificações de email de alertas.  
  
-   Execute BPA em caso de erro/aviso.  
  
-   Configure o System Center Pack de monitoramento.  
  
-   Configure a recuperação de servidor, em caso de corrupção.  
  
 **Experiência do cliente**  
  
-   Implantação de cliente pela internet (PC ou Mac OS).  
  
-   Use barra inicial no cliente para acessar a pasta compartilhada.  
  
-   Ativos de servidor de acesso ao longo de acesso via Web remoto, de dispositivos diferentes (computador pessoal, telefone, tablet).  
  
-   Aplicativo meu servidor para Windows Phone.  
  
-   (Se aplicável) Histórico de arquivos, Backup do cliente e restauração (nenhum BMR), redirecionamento de pasta.  
  
-   (Se aplicável) Experiência de integração de email.  
  
## <a name="where-can-i-get-more-support"></a>Onde posso obter mais suporte?  
 Você pode acessar documentos do SDK e ADK dos links abaixo:  
  
-   [SDK](https://go.microsoft.com/fwlink/p/?LinkID=248648)  
  
-   [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)  
  
 Você pode relatar um bug à equipe de recursos por meio de conexão. Para gerar logs, compactar a pasta no servidor e os clientes ingressar o servidor: C:\ProgramData\Microsoft\Windows Server\Logs.
