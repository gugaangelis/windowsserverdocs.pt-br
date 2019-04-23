---
title: Define o subcomando-servidor
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da55c29d-a94a-4d73-877b-af480f906ca0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abc3fe23558f077e0ba9ac69f2641e3b8c9cde4f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858377"
---
# <a name="subcommand-set-server"></a>Subcommand: set-Server

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define as configurações para um servidor de serviços de implantação do Windows.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Set-Server [/Server:<Server name>]
    [/Authorize:{Yes | No}]
    [/RogueDetection:{Yes | No}]
    [/AnswerClients:{All | Known | None}]
    [/Responsedelay:<time in seconds>]
    [/AllowN12forNewClients:{Yes | No}]
    [/ArchitectureDiscovery:{Yes | No}]
    [/resetBootProgram:{Yes | No}]
    [/DefaultX86X64Imagetype:{x86 | x64 | Both}]
    [/UseDhcpPorts:{Yes | No}]
    [/DhcpOption60:{Yes | No}]
    [/RpcPort:<Port number>]
    [/PxepromptPolicy 
        [/Known:{OptIn | Noprompt | OptOut}]
        [/New:{OptIn | Noprompt | OptOut}] 
    [/BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/N12BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/BootImage:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/PreferredDC:<DC Name>]
    [/PreferredGC:<GC Name>]
    [/PrestageUsingMAC:{Yes | No}]
    [/NewMachineNamingPolicy:<Policy>]
    [/NewMachineOU]
         [/type:{Serverdomain | Userdomain | UserOU | Custom}]
         [/OU:<Domain name of OU>]
    [/DomainSearchOrder:{GCOnly | DCFirst}]
    [/NewMachineDomainJoin:{Yes | No}]
    [/OSCMenuName:<Name>]
    [/WdsClientLogging]
         [/Enabled:{Yes | No}]
         [/LoggingLevel:{None | Errors | Warnings | Info}]
    [/WdsUnattend]
         [/Policy:{Enabled | Disabled}]
         [/CommandlinePrecedence:{Yes | No}]
         [/File:<path>]
             /Architecture:{x86 | ia64 | x64}
    [/AutoaddPolicy]
         [/Policy:{AdminApproval | Disabled}]
         [/PollInterval:{time in seconds}]
         [/MaxRetry:{Retries}]
         [/Message:<Message>]
         [/RetentionPeriod]
             [/Approved:<time in days>]
             [/Others:<time in days>]
    [/AutoaddSettings]
         /Architecture:{x86 | ia64 | x64}
         [/BootProgram:<Relative path>]
         [/ReferralServer:<Server name>
         [/WdsClientUnattend:<Relative path>]
         [/BootImage:<Relative path>]
         [/User:<Owner>]
         [/JoinRights:{JoinOnly | Full}]
         [/JoinDomain:{Yes | No}]
    [/BindPolicy]
         [/Policy:{Include | Exclude}]
         [/add]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
         [/remove]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
    [/RefreshPeriod:<time in seconds>]
    [/BannedGuidPolicy]
         [/add]
              /Guid:<GUID>
         [/remove]
             /Guid:<GUID>
    [/BcdRefreshPolicy]
         [/Enabled:{Yes | No}]
         [/RefreshPeriod:<time in minutes>]
    [/Transport]
         [/ObtainIpv4From:{Dhcp | Range}]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/ObtainIpv6From:Range]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/startPort:<start Port>
             [/EndPort:<start Port>
        [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]
        [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]    
        [/forceNative]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/ Autorizar: {Sim &#124; não}]|Especifica se deve autorizar este servidor no controle de protocolo DHCP (Dynamic Host).|
|[/RogueDetection:{Yes &#124; No}]|Habilita ou desabilita a detecção de invasor do DHCP.|
|[/AnswerClients:{All &#124; Known &#124; None}]|Especifica quais clientes responderá a esse servidor. Se você definir esse valor como **conhecido**, um computador deve ser inserido nos serviços de domínio do Active Directory (AD DS) antes de ele ser respondido pelo servidor de serviços de implantação do Windows.|
|[/Responsedelay:<time in seconds>]|A quantidade de tempo que o servidor aguardará antes de atender a um cliente de inicialização. Essa configuração não se aplica a computadores pré-configurados.|
|[/AllowN12forNewClients:{Yes &#124; No}]|para Windows Server 2008, especifica que os clientes desconhecidos não terá a pressionar a tecla F12 para iniciar uma inicialização de rede. Conhecido, os clientes receberão o programa de inicialização especificado para o computador ou, se não for especificado, o programa de inicialização especificado para a arquitetura.<br /><br />para Windows Server 2008 R2, essa opção foi substituída com o seguinte comando: wdsutil /Set-Server /PxepromptPolicy /New:Noprompt|
|[/ArchitectureDiscovery:{Yes &#124; No}]|Habilita ou desabilita a descoberta da arquitetura. Isso facilita a descoberta de clientes baseados em x64 que não transmita sua arquitetura corretamente.|
|[/resetBootProgram:{Yes &#124; No}]|Determina se o caminho de inicialização será apagado para um cliente que foi inicializado apenas sem a necessidade de um pressionamento de tecla F12.|
|[/DefaultX86X64Imagetype: {x86 &#124; x64 &#124; Both}]|Controla quais imagens de inicialização será mostrada a clientes baseados em x64.|
|[/UseDhcpPorts:{Yes &#124; No}]|Especifica se o servidor PXE deve tentar vincular à porta DHCP, TCP porta 67. Se o DHCP e os serviços de implantação do Windows em execução no mesmo computador, você deve definir essa opção **nenhuma** para habilitar o servidor DHCP utilizar a porta e definir o **/DhcpOption60** parâmetro **Sim**. A configuração padrão para esse valor é **Sim**.|
|[/DhcpOption60:{Yes &#124; No}]|Especifica se a opção 60 do DHCP deve ser configurada para dar suporte a PXE. Se o DHCP e os serviços de implantação do Windows em execução no mesmo servidor, defina essa opção como **Sim** e defina as **/UseDhcpPorts** opção **não**. A configuração padrão para esse valor é **não**.|
|[/RpcPort:<Port number>]|Especifica o número da porta TCP a ser usado para atender a solicitações de cliente.|
|[/PxepromptPolicy]|Configura como conhecidos (inseridos) e uma inicialização PXE de iniciar novos clientes. Essa opção só se aplica ao Windows Server 2008 R2. Definir as configurações usando as seguintes opções:<br /><br />-[Gerais conhecidos: {OptIn&#124;OptOut&#124;Noprompt}]-define a política para clientes pré-testados.<br />-[/ Novo: {OptIn&#124;OptOut&#124;Noprompt}]-define a política para novos clientes.<br /><br />**OptIn** significa que o cliente precisa pressionar uma tecla na ordem de inicialização PXE, caso contrário, ele fará o fallback para o próximo dispositivo de inicialização.<br /><br />**Noprompt** significa que o cliente sempre fará uma inicialização PXE.<br /><br />**OptOut** significa que o cliente fará uma inicialização PXE, a menos que a tecla Esc é pressionada.|
|[/BootProgram:<Relative path>] /Architecture:{x86 &#124; ia64 &#124; x64}|Especifica o caminho relativo para o programa de inicialização na pasta remoteInstall (por exemplo, **boot\x86\pxeboot.n12**) e especifica a arquitetura do programa de inicialização.|
|[/N12BootProgram:<Relative path>] /Architecture:{x86 &#124; ia64 &#124; x64}|Especifica o caminho relativo para o programa de inicialização que não exige o pressionamento da tecla F12 (por exemplo, **boot\x86\pxeboot.n12**) e especifica a arquitetura do programa de inicialização.|
|[/BootImage:<Relative path>] /Architecture:{x86 &#124; ia64 &#124; x64}|Especifica o caminho relativo para a imagem de inicialização que os clientes de inicialização deve receber e especifica a arquitetura da imagem de inicialização. Você pode especificar isso para cada arquitetura.|
|[/PreferredDC:<DC Name>]|Especifica o nome do controlador de domínio dos serviços de implantação do Windows deve usar. Isso pode ser o nome NetBIOS ou FQDN.|
|[/PreferredGC:<GC Name>]|Especifica o nome do servidor de catálogo global dos serviços de implantação do Windows deve usar. Isso pode ser o nome NetBIOS ou FQDN.|
|[/PrestageUsingMAC:{Yes &#124; No}]|Especifica se a serviços de implantação do Windows, ao criar contas de computador no AD DS, deve usar o endereço MAC em vez do GUID/UUID para identificar o computador.|
|[/NewMachineNamingPolicy:<Policy>]|Especifica o formato a ser usado ao gerar nomes de computador para os clientes. Para obter informações sobre o formato a ser usado para <policy>, clique com botão direito do servidor no snap-in do mmc, clique em **propriedades**e exibir os **serviços de diretório** guia. Por exemplo, **/NewMachineNamingPolicy: % 61Username % #**.|
|[/NewMachineOU]|Usado para especificar o local no AD DS em que contas de computador cliente serão criadas. Especifique o local usando as seguintes opções.<br /><br />-   [/type: Serverdomain &#124; Userdomain &#124; unidade organizacional do usuário &#124; personalizado] Especifica o tipo de local. **Serverdomain** cria contas no mesmo domínio que o servidor de serviços de implantação do Windows. **USERDOMAIN** cria contas no mesmo domínio que o usuário que está executando a instalação. **Unidade organizacional do usuário** cria contas na unidade organizacional do usuário que está executando a instalação. **Personalizado** permite que você especifique um local personalizado (você também deve especificar um valor para **/OU** com essa opção).<br />-[/ UO:<Domain name of OU>] – se você especificar **personalizado** para o **/tipo** opção, essa opção especifica a unidade organizacional em que as contas de computador devem ser criadas.|
|[/DomainSearchOrder:{GCOnly &#124; DCFirst}]|Especifica a política para a pesquisa de contas de computador no AD DS (global catálogo ou controlador de domínio).|
|[/NewMachineDomainJoin:{Yes &#124; No}]|Especifica se um computador que não tenha sido pré-testado já no AD DS deve ser ingressado no domínio durante a instalação. A configuração padrão é **Sim**.|
|[/WdsClientLogging]|Especifica o nível de log para o servidor.<br /><br />-[/ Habilitado: {Sim &#124; não}] - habilita ou desabilita o log de ações do cliente dos serviços de implantação do Windows.<br />-[/ LoggingLevel: {None &#124; erros &#124; avisos &#124; Info}-define o nível de log. **Nenhum** é equivalente ao desabilitar o registro em log. **Erros** é o menor nível de registro em log e indica que somente os erros serão registrados. **Avisos** inclui avisos e erros. **Informações de** é o maior nível de registro em log e inclui erros, avisos e eventos informativos.|
|[/WdsUnattend]|Essas configurações controlam o comportamento de instalação autônoma do cliente dos serviços de implantação do Windows. Definir as configurações usando as seguintes opções:<br /><br />-[/ Política: {habilitado &#124; desabilitado}]-Especifica se a instalação autônoma é usada.<br />-[/ CommandlinePrecedence: {Sim &#124; não}]-Especifica se um arquivo Autounattend XML (se presente no cliente) ou um arquivo de instalação autônoma que foi passado diretamente para o cliente de serviços de implantação do Windows com a opção /Unattend será usado em vez de um arquivo autônomo de imagem durante uma instalação do cliente. A configuração padrão é **não**.<br />-[Arquivos:<Relative path> /Architecture: {x86 &#124; ia64 &#124; x64}]-Especifica o nome do arquivo, o caminho e a arquitetura do arquivo autônomo.|
|[/AutoaddPolicy]|Essas configurações controlam a política de adição automática. Você define as configurações usando as seguintes opções:<br /><br />-[/ Política: {AdminApproval &#124; desabilitado}]- **AdminApprove** faz com que todos os computadores desconhecidos a ser adicionado a uma fila pendente, em que o administrador pode, em seguida, examine a lista de computadores e aprovar ou rejeitar cada solicitação, como apropriado. **Desabilitado** indica que nenhuma ação adicional é executada quando um computador desconhecido tenta é inicializado para o servidor.<br />-[/ Intervalopesquisa: {tempo em segundos}]-Especifica o intervalo (em segundos) no qual o programa de inicialização de rede deve sondar o servidor de serviços de implantação do Windows.<br />-[/ MaxRetry: <Number>]-Especifica o número de vezes que o programa de inicialização de rede deve sondar o servidor de serviços de implantação do Windows. Esse valor, juntamente com **/PollInterval**, determina por quanto tempo o programa de inicialização de rede irá aguardar um administrador aprovar ou rejeitar o computador antes do tempo limite. Por exemplo, uma **MaxRetry** valor de 10 e uma **PollInterval** vlue de 60 indicaria que o cliente deve sondar o servidor de 10 vezes, aguardando a 60 segundos entre tentativas. Portanto, o cliente será o tempo limite após 10 minutos (10 x 60 segundos = 10 minutos).<br />-[/ Mensagem: <Message>]-Especifica a mensagem que é exibida para o cliente na página de diálogo do programa de inicialização de rede.<br />-[/RetentionPeriod] - Especifica o número de dias que um computador pode ficar em um estado pendente antes de ser limpos automaticamente.<br />-[/ Aprovado: <time in days>]-Especifica o período de retenção para computadores aprovados. Você deve usar esse parâmetro com o **/RetentionPeriod** opção.<br />-[/ Outras pessoas: <time in days>]-Especifica o período de retenção para computadores não aprovados (rejeitado ou pendente). Você deve usar esse parâmetro com o **/RetentionPeriod** opção.|
|[/AutoaddSettings]|Especifica as configurações padrão a ser aplicado a cada computador. Você define as configurações usando as seguintes opções:<br /><br />-/Architecture: {x86 &#124; ia64 &#124; x64}-Especifica a arquitetura.<br />-[/ BootProgram: <Relative path>]-Especifica o programa de inicialização enviado para o computador aprovado. Se nenhum programa de inicialização for especificado, o padrão para a arquitetura do computador (como especificado no servidor) será usado.<br />-[/ WdsClientUnattend: <Relative path>]-define o caminho relativo para o arquivo autônomo que o cliente aprovado deve receber.<br />-[/ ReferralServer: <Server name>]-Especifica o servidor de serviços de implantação do Windows que o cliente usará para fazer o download de imagens.<br />-[/ Imagem de inicialização: <Relative path>]-Especifica a imagem de inicialização que receberá o cliente aprovado.<br />-[/ Usuário: < domínio \ usuário &#124; User@Domain>]-define as permissões no objeto de conta de computador para que o usuário especificado os direitos necessários para ingressar o computador ao domínio.<br />-[JoinRights: {JoinOnly &#124; completo}]-Especifica o tipo de direitos a ser atribuído ao usuário. **JoinOnly** exige que o administrador redefinir a conta de computador antes do usuário pode ingressar o computador ao domínio. **Completo** fornece acesso completo para o usuário, inclusive o direito de ingressar o computador no domínio.<br />-[/ JoinDomain: {Sim &#124; não}]-Especifica se o computador deve estar associado ao domínio como esta conta de computador durante uma instalação de serviços de implantação do Windows. A configuração padrão é **Sim**.|
|[/BindPolicy]|Configura as interfaces de rede para o provedor PXE escutar em. Você define a política usando as seguintes opções:<br /><br />-[/ Política: {Include &#124; excluir}]-define a política de associação de interface para incluir ou excluir os endereços na lista de interface.<br />-[/Add] - adiciona uma interface à lista. Você também deve especificar /addresstype e /address.<br />-[/remove] - remove uma interface da lista. Você também deve especificar /addresstype e /address.<br />- endereço:<IP or MAC address> -Especifica o endereço IP ou MAC da interface para adicionar ou remover.<br />-/addresstype: {IP &#124; MAC}-indica o tipo de endereço especificado a **/endereço** opção.|
|[/ RefreshPeriod: <seconds>]|Especifica a frequência (em segundos) será o servidor de atualização das configurações.|
|[/BannedGuidPolicy]|Gerencia a lista de GUIDs banidas usando as seguintes opções:<br /><br />-[/ Adicionar] /Guid:<GUID> -adiciona o GUID especificado à lista de GUIDs banidas. Em vez disso, qualquer cliente com esse GUID será identificada por seu endereço MAC.<br />-/Guid [/remove]:<GUID> -remove o GUID especificado da lista de GUIDs banidas.|
|[/BcdRefreshPolicy]|Define as configurações para atualizar os arquivos de Bcd usando as seguintes opções:<br /><br />-[/ Habilitado: {Sim &#124; não}]-Especifica o Bcd atualizando a política. Quando **/habilitado** é definido como **Sim**, arquivos Bcd são atualizados no intervalo de tempo especificado.<br />-[/ RefreshPeriod:<time in minutes>]-Especifica o intervalo de tempo no qual Bcd arquivos são atualizados.|
|[/Transport]|Configura as opções a seguir:<br /><br /><ul><li>[/ ObtainIpv4From: {Dhcp &#124; intervalo}]-Especifica a origem dos endereços IPv4.<br /><br /><ul><li>[/ Iniciar: <starting Ipv4 address>]-Especifica o início do intervalo de endereços IP. Essa opção é necessária e válida somente se **/ObtainIpv4From** é definido como **intervalo**</li><li>[Final: <Ending Ipv4 address>]-Especifica o final do intervalo de endereços IP. Essa opção é necessária e válida somente se **/ObtainIpv4From** é definido como **intervalo**.</li></ul></li><li>[/ ObtainIpv6From:Range] [/ Iniciar:<start IP address>] [/ fim:<End IP address>] Especifica a origem dos endereços IPv6. Essa opção só se aplica ao Windows Server 2008 R2 e o único valor aceito é um intervalo.</li><li>[/ startPort: <starting port>]-Especifica o início do intervalo de porta.</li><li>[/ A EndPort: <Ending port>]-Especifica o final do intervalo de porta.</li><li>[/Profile: {10Mbps &#124; 100 Mbps &#124; 1 Gbps &#124; personalizado}]-Especifica o perfil de rede a ser usado. Essa opção é somente com suporte forservers executando o Windows Server 2008.</li><li>[/MulticastSessionPolicy]  Define as configurações de transferência para transmissões multicast. Esse comando só está disponível para Windows Server 2008 R2.<br /><br /><ul><li>[/ Política: {None &#124; desconexão automática &#124; Multistream}]-determina como lidar com clientes lentos. Nenhum significa manter todos os clientes em uma sessão com a mesma velocidade. Desconexão automática significa que todos os clientes que atingem o /Threshold especificado serão desconectados. MultiStream significa que os clientes serão separados em várias sessões conforme especificado pelo /StreamCount.</li><li>[/ Limite:<Speed in KBps>] - para /Policy:AutoDisconnect, esses conjuntos de opção de taxa de transferência mínima em KBps. Os clientes que atingem essa taxa serão desconectados do transmissões multicast.</li><li>[/ StreamCount: {2 &#124; 3}] [/ Fallback: {Sim &#124; não}]-para /Policy:Multistream, essa opção determina o número de sessões. 2 significa que duas sessões (rápidas e lentas) 3 significa três sessões (lenta, média, fast).</li><li>[/ Fallback: {Sim&#124; não}]-determina se os clientes que estão desconectados continuarão a transferência usando outro método (se houver suporte pelo cliente). Se você estiver usando o cliente do WDS, o computador fará fallback para unicast. Wdsmcast.exe não oferece suporte a um mecanismo de fallback. Essa opção também se aplica aos clientes que não dão suporte a fluxo múltiplo. Nesse caso, o computador fará o fallback para outro método em vez de mover para uma sessão de transferência mais lenta.</li></ul></li></ul>|
## <a name="BKMK_examples"></a>Exemplos
Para definir o servidor responder apenas clientes conhecidos, com um atraso de resposta de 4 minutos, digite:
```
wdsutil /Set-Server /AnswerClients:Known /Responsedelay:4
```
Para definir o programa de inicialização e a arquitetura para o servidor, digite:
```
wdsutil /Set-Server /BootProgram:boot\x86\pxeboot.n12 /Architecture:x86
```
Para habilitar o registro em log no servidor, digite:
```
wdsutil /Set-Server /WdsClientLogging /Enabled:Yes /LoggingLevel:Warnings
```
Para habilitar autônomos no servidor, bem como a arquitetura e o arquivo autônomo do cliente, digite:
```
wdsutil /Set-Server /WdsUnattend /Policy:Enabled /File:WDSClientUnattend \unattend.xml /Architecture:x86
```
Para definir o servidor Pre-Boot execution Environment (PXE) para tentar se associar às portas TCP 67 e 60, digite:
```
wdsutil /Set-server /UseDhcpPorts:No /DhcpOption60:Yes
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando de servidor disable](using-the-disable-server-command.md)
[usando o comando enable-Server](using-the-enable-server-command.md)
[usando o Comando Get-Server](using-the-get-server-command.md)
[usando o comando do servidor de inicialização](using-the-initialize-server-command.md)
[subcomando: Iniciar servidor](subcommand-start-server.md) 
 [ Subcomando: stop-Server](subcommand-stop-server.md)
[o opção de servidor uninitialize](the-uninitialize-server-option.md)
