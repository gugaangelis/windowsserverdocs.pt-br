---
title: Windows Server Essentials hospedado
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
ms.openlocfilehash: 1b78432ca92028bc96b2cbfc9fa40196f61e8bf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833697"
---
# <a name="hosted-windows-server-essentials"></a>Windows Server Essentials hospedado

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Este documento inclui informações específicas a hosters que pretendem implantar o Windows Server Essentials no laboratório e oferecer aos clientes Windows Server Essentials como um serviço.  
  
## <a name="what-is-windows-server-essentials"></a>O que é o Windows Server Essentials?  
 Windows Server Essentials é uma solução para pequenas empresas entre locais, que incorpora as tecnologias do produto de ponta, 64 bits para entregar um ambiente de servidor adequado para a maioria das pequenas empresas. As tecnologias a seguir estão incluídas no Windows Server Essentials.  
  
 **Sistema operacional de servidor:** As tecnologias do produto Windows Server 2012 fornecem o centro do Windows Server Essentials. Para mais informações, visite o site do [Windows Server 2012](https://www.microsoft.com/en-us/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh).  
  
 **Proteção de dados:** Windows Server Essentials aproveita vários novos recursos disponíveis no Windows Server 2012 para fornecer recursos de proteção de dados muito aprimorados. O [novo recurso de Espaços de Armazenamento](https://technet.microsoft.com/library/hh831739.aspx) permite agregar a capacidade de armazenamento físico de discos rígidos diferentes, adicionar discos rígidos dinamicamente e criar volumes de dados com níveis especificados de resiliência. Windows Server Essentials pode realizar backups completos do sistema e restaurações bare-metal do servidor em si, bem como os computadores cliente conectados à rede? agora com suporte para volumes maiores que 2 TB. Novo no Windows Server 2012, o [Microsoft Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx) pode ser usado para proteger arquivos e pastas em um serviço de armazenamento baseado em nuvem gerenciado pela Microsoft. Windows Server Essentials também centralmente gerencia e configura o recurso de histórico de arquivos de clientes do Windows 8.1, ajudando os usuários a recuperar arquivos acidentalmente excluídos ou substituídos sem exigir assistência do administrador.  
  
 **Acesso em qualquer local:** O acesso remoto via Web fornece uma experiência de navegador simplificada e compatível com toque para acessar aplicativos e dados de praticamente qualquer lugar em que haja uma conexão com a Internet e usando quase qualquer dispositivo. Windows Server Essentials também fornece um aplicativo atualizado do Windows Phone e um novo aplicativo para Windows 8.1 cliente computadores, permitindo que os usuários intuitivamente conectar-se para pesquisar os e acessar arquivos e pastas no servidor. Os arquivos são automaticamente armazenados em cache para acesso offline e sincronizados quando uma conexão ao servidor está disponível. Windows Server Essentials transforma configurar a rede virtual privada (VPN) em um processo simples e orientada por assistente, de apenas alguns cliques e simplifica o gerenciamento de acesso VPN para usuários. Os computadores cliente podem aproveitar uma conexão VPN para entrarem remotamente para o ambiente Windows SBS sem necessidade de irem para o escritório.  
  
 **Flexibilidade de carga de trabalho:** Windows Server Essentials foi projetado para permitir aos clientes a flexibilidade de escolher quais aplicativos e serviços executar nas instalações e quais executar na nuvem. Em versões anteriores, o Windows Small Business Server Standard incluía o Exchange Server como um produto de componente, que adicionava despesa e complexidade aos clientes que desejavam aproveitar serviços de colaboração e mensagens baseados na nuvem. Com o Windows Server Essentials, os clientes podem aproveitar o mesmo tipo de experiência de gerenciamento integrado quando eles optam por executar uma cópia local do Exchange Server, assinar um serviço hospedado do Exchange ou assinar o Microsoft Office 365.  
  
 **Monitoramento de integridade:** Windows Server Essentials monitora o próprio status de integridade e o status dos computadores cliente executando Windows 8.1, Windows 7 e Mac OS X versão 10.5 e posteriores. O status de integridade notifica-o das questões ou problemas relacionados a backups de computador, armazenamento de servidor, pouco espaço em disco, entre outros.  
  
 **Extensibilidade:** Windows Server Essentials desenvolve o modelo de extensibilidade do Windows SBS 2011 Essentials, que permite que outros fornecedores de software adicionar capacidades e recursos ao produto central e adiciona um novo conjunto de APIs de serviços da web. Também mantém compatibilidade com o [kit de desenvolvimento de software](https://msdn.microsoft.com/library/gg513958.aspx) (SDK) existente e com [complementos](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials) criados para o Windows SBS 2011 Essentials.  
  
## <a name="how-can-i-customize-an-image"></a>Como posso personalizar uma imagem?  
 Consulte a [Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124), que é um processo de sysprep padrão do Windows Server com etapas de personalização adicionais do Windows Server Essentials. Para concluir a personalização, siga as instruções em [Criar uma Imagem Simples Personalizada](https://technet.microsoft.com/library/jj200117) e [Personalizar a Imagem](https://technet.microsoft.com/library/jj200161), então siga as instruções em [Preparando a Imagem para Implantação](https://technet.microsoft.com/library/jj200142) para capturar sua imagem final.  
  
 É preciso prestar atenção aos seguintes pontos:  
  
1.  Você deve ignorar a Configuração Inicial (IC) adicionando um arquivo SkipIC.txt à raiz de qualquer unidade. Depois de instalar o servidor, antes da IC, pressione Shift+F10 para iniciar a janela cmd e criar um arquivo SkipIC.txt sob a unidade C:/. Depois da personalização, você deve lembrar de excluir o arquivo SkipIC.txt.  
  
2.  Se você precisar implantar o Windows Server Essentials em um disco menos de 90 GB, você deve adicionar uma chave do registro antes do sysprep:  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
 Depois do sysprep, é possível usar a imagem de disco preparada para o sistema, ou recolocá-la em Install.wim para nova implantação.  
  
 Se estiver usando o Virtual Machine Manager, pode criar um modelo usando a instância em execução. Criar um modelo irá realizar a preparação do sistema da instância e desligará o servidor. Depois de armazená-la na sua biblioteca, é possível abrir a instância caso a caso.  
  
##  <a name="BKMK_automatedeployment"></a> Como automatizar a implantação?  
 Depois de obter uma imagem personalizada, é possível realizar a implantação com a sua própria imagem. Para realizar a instalação parcialmente não monitorada, é preciso fornecer/implantar o unattend.xml para a instalação do WinPE. Para fazer uma instalação totalmente autônoma, você também precisará fornecer o arquivo cfg. ini para configuração inicial do Windows Server Essentials.  
  
1.  Realizar somente instalação não monitorada do WinPE. Isso automatizará somente a instalação do WinPE, e permitirá que a instalação pare antes da Configuração inicial de modo que os usuários finais possam fornecer informações de Corporação, Domínio e Administrador por si mesmos após o RDP na sessão no servidor. Para fazer isso:  
  
    1.  Forneça o arquivo unattend.xml do Windows. Siga as [Windows 8.1 ADK](https://go.microsoft.com/fwlink/?LinkId=248694) para gerar o arquivo e forneça todas as informações necessárias, incluindo o nome do servidor, chaves do produto e senha de administrador. Na seção Microsoft-Windows-Setup do arquivo Unattend. XML, forneça as informações conforme mostrado a seguir.  
  
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
  
    2.  A porta 3389 RDP deve ser aberta em um IP público para que o cliente pode usar o administrador e a senha especificada no arquivo Unattend. XML para RDP no servidor para concluir a configuração inicial.  
  
    > [!NOTE]
    >  Se você não alterar a senha padrão, a instalação do servidor irá parar em uma tela pedindo a digitação de uma senha.**Observação** Os usuários finais devem usar a conta de administrador padrão para realizarem logon no servidor e Configuração Inicial.  
  
 Se você estiver usando o Gerenciador de Máquina Virtual, pode especificar a senha do administrador no console quando criar uma nova instância do modelo.  
  
1.  Realizar configuração completa não monitorada, incluindo Configuração Inicial não monitorada. Para fazer isso:  
  
    1.  Forneça o arquivo unattend.xml como fez acima, se a implementação iniciar a partir da instalação do WinPE.  
  
    2.  Consulte a seção do Windows Server Essentials ADK chamada, [criar o arquivo cfg. ini](https://technet.microsoft.com/library/jj200150), para gerar o cfg.  
  
    3.  Fornecer informações em [InitialConfiguration].  
  
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
  
    4.  Fornecer informações em [PostOSInstall].  
  
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
  
    5.  Se você fornecer o parâmetro WebDomainName, certifique-se de que o registro DNS também está sendo atualizado para apontar para o IP público do servidor s.  
  
    6.  Se você não forneceu as informações de WebDomainName acima, certifique-se de abrir a porta 3389 de modo que os clientes possam usar o RDP para conectarem-se ao servidor e concluírem as configurações de VPN.  
  
> [!NOTE]
>  Certifique-se de que a configuração de fuso horário do servidor de host VM e VM do Windows Server Essentials é o mesmo. Caso contrário, podem ocorrer vários erros diferentes (a Configuração Inicial pode falhar em tarefas relacionadas a certificado; o certificado pode não funcionar por algumas horas após a instalação; as informações do dispositivo não atualizarão corretamente; e assim por diante).  
  
 Após a implantação, verifique a seguinte chave de registro sob HKLM\software\microsoft\windows server\setup para verificar se a Configuração Inicial foi bem-sucedida. Se SetupStage == ICDone && ICStatus == 1, isso significa que a Configuração Inicial foi concluída com sucesso.  
  
## <a name="what-is-the-supported-network-topology"></a>O que é a topologia de rede suportada?  
 Para usar o Windows Server Essentials em um cliente de roaming, o VPN deve ser habilitado. Recomendamos habilitar a porta 443 para conexões SSTP de VPN. Se o recurso de Servidor de Mídia for necessário para Acesso Remoto da Web ou aplicativos de Serviço da Web, a porta 80 também deve ser habilitada.  
  
 A habilitação de VPN pode ser feita durante a implantação não monitorada via nosso script Windows PowerShell, ou pode ser configurada com nosso assistente após a configuração inicial.  
  

-   Para habilitar a VPN durante a implantação autônoma, consulte [How do I automate the deployment?](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) neste documento.  

-   Para habilitar a VPN durante a implantação autônoma, consulte [How do I automate the deployment?](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) neste documento.  

  
-   Para habilitar VPN via Windows PowerShell, execute o seguinte cmdlet com privilégio administrativo e forneça todas as informações necessárias.  
  
    ```  
    ##  
    ## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
    ##  
  
    $myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
    $mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
    $mySslCertificatePassword = ConvertTo-SecureString '******';   ## password for private key of the SSL certificate  
    $skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
    Add-Type -AssemblyName 'Wssg.Web.DomainManagerObjectModel';  
    [Microsoft.WindowsServerSolutions.RemoteAccess.Domains.DomainConfigurationHelper]::SetDomainNameAndCertificate($myExternalDomainName,$mySslCertificateFile,$mySslCertificatePassword,$skipCertificateVerification);  
    ##  
    ## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
    ##  
  
    Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
    ```  
  
 Se você não puder fornecer uma conexão VPN antes de entregar o servidor aos clientes, garanta que a porta 3389 do servidor possa ser acessada pela Internet para que os usuários finais possam usar RDP para conexão com o servidor e para realizar a configuração por si mesmos.  
  
 Aqui estão duas topologias de sistema de rede do lado do servidor típicas e como o VPN/Acesso Remoto da Web (RWA) poderia ser configurado:  
  
-   Topologia 1 (preferida)  
  
    -   Servidor em uma rede virtual separada sob um dispositivo NAT.  
  
    -   O serviço DHCP é habilitado na rede virtual, ou o servidor tem um endereço IP estático redesignado.  
  
    -   A porta 443 do servidor pode ser acessada pela porta 443 IP pública.  
  
    -   Passagem VPN é permitida para a porta 443.  
  
    -   O pool de endereços VPN IPv4 deve estar no alcance da mesma sub-rede do endereço do servidor.  
  
    -   Qualquer segundo servidor deve ter um IP estático designado dentro da mesma subrede, mas fora do pool de endereço da VPN.  
  
-   Topologia 2:  
  
    -   O servidor tem um endereço IP privado.  
  
    -   Porta 443 no servidor está acessível a partir de uma endereço s porta 443 IP pública.  
  
    -   Passagem VPN é permitida para a porta 443.  
  
    -   O pool de endereços VPN IPv4 está em um alcance diferente do endereço do servidor.  
  
 Com a Topologia 2, não há suporte para cenários de segundo servidor.  
  
## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>Como eu realizo tarefas comuns via Windows PowerShell?  
 **Habilitar o acesso via Web remoto**  
  
```  
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
```  
  
 Exemplo:  
  
```  
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
```  
  
 Esse comando permitirá Acesso Remoto da Web com o roteador configurado automaticamente, e trocará as permissões de acesso padrão para todos os usuários existentes.  
  
 **Adicionar usuário**  
  
```  
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]  
```  
  
 Exemplo:  
  
```  
$password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce  
$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
```  
  
 Esse comando adicionará um administrador chamado User2Test com senha Passw0rd!.  
  
 **Habilitar/desabilitar usuário**  
  
 Exemplo:  
  
```  
$CurrentUser = get-wssuser  œname user2test  
$CurrentUser.UserStatus = 0  
$CurrentUser.Commit()  
```  
  
 Este comando desabilitará o usuário chamado user2test. Configurar UserStatus para 1 habilitará o usuário.  
  
 **Adicionar pasta do servidor**  
  
```  
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]  
```  
  
 Exemplo:  
  
```  
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
```  
  
 Esse comando adicionará uma pasta de servidor chamada MyTestFolder no local especificado.  
  
## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>Como posso adicionar um segundo servidor ao domínio do Windows Server Essentials?  
 Como o Windows Server Essentials é um controlador de domínio, você pode adicionar um segundo servidor ao domínio da maneira padrão.  
  
## <a name="which-email-solutions-can-be-integrated"></a>Que soluções de email podem ser integradas?  
 Windows Server Essentials oferece suporte à integração com duas soluções de email fora da caixa: Office 365 e Exchange local. Se você estiver executando sua própria solução de email hospedado, você precisará desenvolver um suplemento para integrar o Windows Server Essentials com sua solução de email hospedado.  
  
## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>Como posso migrar local Windows SBS (2008/2011/2003) para o Windows Server Essentials hospedado?  
 Guias de migração estão disponíveis para o local Windows Small Business Server (Windows SBS) para as migrações do Windows Server Essentials. Algumas das etapas podem não se aplicar exatamente da mesma maneira ao seu ambiente hospedado. Entretanto, as tarefas gerais e cargas de trabalho a serem migradas devem ser iguais. Recomendamos que você consulte os [guias de migração](https://go.microsoft.com/fwlink/p/?LinkID=254292) e faça as personalizações necessárias com base no seu ambiente de hospedagem.  
  
 Recomenda-se colocar o servidor de origem e o servidor de destino na mesma sub-rede. Se isso não for possível, você deve garantir que:  
  
-   O nome de DNS interno do servidor de origem e do servidor de destino possam ser acessados um pelo outro.  
  
-   Todas as portas necessárias estejam abertas.  
  
## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>Como atualizar o Windows Server Essentials para o Windows Server Standard?  
 Você pode atualizar o Windows Server Essentials para Windows Server Standard. Remova bloqueios e limites e adicione os pacotes que estão faltando do Windows Server Standard. Para mais informações, [baixe o documento](https://go.microsoft.com/fwlink/p/?LinkID=253181).  
  
## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>Quais são as ferramentas nativas para monitoramento e gerenciamento?  
  
### <a name="group-policy-management"></a>Gerenciamento de Política de Grupo  
 Windows Server Essentials aproveita o suporte nativo de política de grupo no Windows Server 2012 e fornece a interface do usuário para definir as configurações de segurança e redirecionamento de pasta.  
  
> [!NOTE]
>  Em um ambiente hospedado, se o redirecionamento de pasta para um perfil de usuário estiver habilitado, isso pode aumentar o tempo para os usuários finais efetuarem logon quando o tamanho dos dados for grande.  
  
### <a name="management-pack"></a>Pacote de gerenciamento  
 Pacote de gerenciamento do Windows Server Essentials fornece função de monitoramento sobre o sistema de alerta de integridade no Windows Server Essentials para ajudar os hosters gerenciar um grande número de servidores do Windows Server Essentials dedicados a diferentes pequenas empresas. O monitoramento nessa versão inclui somente alertas críticos no sistema.  
  
#### <a name="management-pack-scope"></a>Escopo do pacote de gerenciamento  
 Este pacote de gerenciamento ajuda a monitorar recursos específicos do Windows Server Essentials. Ele não monitora recursos genéricos no sistema operacional Windows Server 2012 Standard. Para monitorar o Windows Server Essentials, você deve usar o pacote de gerenciamento do Windows Server Essentials e o Management Pack para Windows Server 2012 Standard.  
  
#### <a name="mandatory-configuration"></a>Configuração obrigatória  
 As seguintes etapas precisam ser tomadas antes de você poder usar o pacote de gerenciamento:  
  
1.  Instalar o Agente e configurar a confiança usando a confiança de certificado. Como o Windows Server Essentials é pré-configurado como um controlador de domínio e não pode ter confiança com outros domínios ou florestas, o agente do System Center Operations Manager deve ser instalado no Windows Server Essentials e confiança configurada com o gerenciamento usando certificados de servidor.  
  
2.  Baixe o pacote de gerenciamento. Para monitorar o Windows Server Essentials usando o Operations Manager 2007, você deve primeiro baixar o [pacote de gerenciamento de sistema operacional Windows Server](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010) do catálogo de pacote de gerenciamento.  
  
3.  Importar arquivo de pacote de gerenciamento. Se você estiver usando uma versão localizada do pacote de gerenciamento, é preciso importar o arquivo do pacote de gerenciamento principal e o pacote de idiomas.  
  
#### <a name="files-in-this-monitoring-pack"></a>Arquivos nesse Pacote de Monitoramento  
 O monitoramento de pacote para o Windows Server Essentials inclui os seguintes arquivos:  
  
-   Microsoft.Windows.Server.2012.Essentials.mp  
  
-   Microsoft.Windows.Server.2012.Essentials.<locale\>.mp  
  
### <a name="back-up-and-restore"></a>Fazer backup e restaurar  
 Windows Server Essentials permite que você faça backup do servidor e o cliente.  
  
#### <a name="back-up-the-server"></a>Faça o backup do servidor  
 Windows Server Essentials oferece suporte a duas maneiras de realizar backup do servidor: backup local e backup externo.  
  
 O**backup local** permite que você realize backup incremental no nível do bloco regularmente em um disco separado. Como hoster, você pode anexar um disco virtual à VM do Windows Server Essentials e configurar o backup do servidor nesse disco virtual. O disco virtual deve estar localizado em um disco físico diferente de VM do Windows Server Essentials.  
  
-   Se você tiver outro mecanismo para fazer backup de VM do Windows Server Essentials e você não deseja que o usuário veja o recurso de Backup do servidor nativo do Windows Server Essentials, você pode desativá-lo e remover todas as interfaces de usuário relacionado do Windows Server Essentials Painel de controle. Para obter mais informações, consulte a seção personalizar Backup do servidor das [documento ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
 O **Backup externo** permite periodicamente realizar backup de dados do servidor em um serviço de nuvem. Você pode baixar e instalar o Microsoft Azure Backup Integration Module para Windows Server Essentials para aproveitar o Azure Backup fornecidas pela Microsoft.  
  
 Se você ou seus usuários preferirem outro serviço na nuvem, você deve:  
  
1.  Atualize a interface do usuário do painel do Windows Server Essentials, para que ele fornece um link para seu serviço de nuvem preferido, em vez do padrão de Backup do Azure. Para mais informações, consulte a seção Personalizar a Imagem do [documento ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
2.  (Opcional) Desenvolva um suplemento para o painel do Windows Server Essentials configurar e gerenciar o serviço de backup de nuvem.  
  
#### <a name="back-up-the-client"></a>Faça o backup do cliente  
 Windows Server Essentials oferece suporte a dois tipos de backup de dados do cliente: backup completo do cliente e o histórico de arquivos.  
  
> [!NOTE]
>  Realizar o backup do cliente pode ter impacto sobre o desempenho, pois os dados precisam ser transferidos do cliente para o servidor por VPN.  
  
 **Backup completo do cliente** destina-se por padrão em todos os dispositivos de cliente conectados à rede Windows Server Essentials. Faz backup do cliente completo (sistema e dados) incrementalmente e suporta eliminação de duplicação de dados. Os dados de backup será no servidor executando o Windows Server Essentials. Um cliente com falha pode obter seus dados para um ponto de backup anterior. Você pode desativar esse recurso seguindo as etapas no criar a seção do arquivo cfg. ini do [documento ADK](https://technet.microsoft.com/library/jj200150).  
  
 Algumas considerações para backup completo do cliente são:  
  
-   Desempenho: o backup inicial do cliente pode ser demorado em função da quantidade de dados a serem carregados.  
  
-   Estabilidade: às vezes a conexão com a Internet não é estável no lado do cliente. O backup do cliente é projetado para ser reiniciável, e o ponto de verificação padrão é de 40 GB (o banco de dados de backup do cliente criará um ponto de verificação sempre que 40 GB de dados tiverem passado por backup). Você pode alterar esse valor para um número menor se for esperado que a conexão com a Internet seja pouco confiável.  
  
    -   Para habilitar um trabalho de ponto de verificação, no servidor, defina a chave de registro HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs para 1.  
  
    -   Para alterar o limite do ponto de verificação, no cliente, altere HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold do seu valor padrão (40 GB).  
  
-   Restauração Bare Metal do Cliente: porque o Ambiente de Pré-instalação do Windows não tem suporte para conexão VPN, não há suporte para a Restauração Bare Metal do Cliente.  
  
 **Histórico de arquivos** é um recurso do Windows 8.1 para fazer backup dos dados de perfil (bibliotecas, área de trabalho, contatos, Favoritos) para um compartilhamento de rede. No Windows Server Essentials, permitimos o gerenciamento central da configuração do histórico de arquivos de todos os clientes do Windows 8.1 ingressados no Windows Server Essentials. Os dados de backup são armazenados no servidor executando o Windows Server Essentials. Você pode desativar esse recurso seguindo as etapas no criar a seção do arquivo cfg. ini do [documento ADK](https://technet.microsoft.com/library/jj200150).  
  
### <a name="storage-management"></a>Gerenciamento de armazenamento  
 O [novo recurso de Espaços de Armazenamento](https://technet.microsoft.com/library/hh831739.aspx) permite agregar a capacidade de armazenamento físico de discos rígidos diferentes, adicionar discos rígidos dinamicamente e criar volumes de dados com níveis especificados de resiliência. Você também pode anexar um disco iSCSI ao Windows Server Essentials para expandir o armazenamento.  
  
## <a name="what-are-the-main-scenarios-i-should-test"></a>Quais são os cenários principais que eu devo testar?  
 Da perspectiva de hospedagem, recomenda-se testar os seguintes cenários:  
  
 **Implantação de servidor**  
  
-   Implante o servidor do Windows Server Essentials em seu ambiente de laboratório.  
  
-   Personalize a imagem do Windows Server Essentials conforme necessário.  
  
-   Automatize a implantação do Windows Server Essentials com arquivo e cfg.  
  
-   Migre local Windows SBS para o Windows Server Essentials hospedado.  
  
-   Atualização do Windows Server Essentials para o Windows Server 2012.  
  
 **Configuração do servidor**  
  
-   Configurar o acesso em qualquer local (VPN, Acesso Remoto via Web, DirectAccess).  
  
-   Configurar a Pasta de Armazenamento e Servidor.  
  
-   (Se aplicável) Configurar Backup de Servidor, Online Backup, Backup do Cliente, Histórico de Arquivos.  
  
-   (Se aplicável) Configurar e gerenciar os Espaços de Armazenamento.  
  
-   (Se aplicável) Configurar a integração da solução de email (Office 365, Exchange hospedado, e assim por diante).  
  
-   (Se aplicável) Configurar o Servidor de Mídia.  
  
 **Gerenciamento de servidor**  
  
-   Gerenciar usuários.  
  
-   Configurar e receber notificações de alertas por email.  
  
-   Executar o BPA no caso de erro/aviso.  
  
-   Configurar o pacote de monitoramento do System Center.  
  
-   Configurar a recuperação do servidor, no caso de corrompimento.  
  
 **Experiência do cliente**  
  
-   Implantação do cliente pela Internet (PC ou Mac OS).  
  
-   Usar a Barra Inicial no cliente para acessar a Pasta Compartilhada.  
  
-   Acessar ativos do servidor por acesso remoto via Web de diferentes dispositivos (PC, telefone, tablet).  
  
-   Aplicativo My Server para Windows Phone.  
  
-   (Se aplicável) Histórico de Arquivos, Backup e Restauração do Cliente (sem BMR), Redirecionamento de Pasta.  
  
-   (Se aplicável) Experiência de integração de email.  
  
## <a name="where-can-i-get-more-support"></a>Onde posso obter mais suporte?  
 Você pode obter os documentos do SDK e do ADK nos links abaixo:  
  
-   [SDK](https://go.microsoft.com/fwlink/p/?LinkID=248648)  
  
-   [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)  
  
 Você pode relatar um erro à equipe de recursos através do Connect. Para gerar logs, compacte a pasta no servidor e nos clientes que fazem parte do servidor: C:\ProgramData\Microsoft\Windows Server\Logs.
