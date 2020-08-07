---
title: Conjunto de subcomandos-servidor
description: Artigo de referência para o subcomando set-Server, que definiu as configurações para um servidor de serviços de implantação do Windows.
ms.topic: article
ms.assetid: da55c29d-a94a-4d73-877b-af480f906ca0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aa9a40be9b2af534ddf80b03e2c56ac06b533a75
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882132"
---
# <a name="subcommand-set-server"></a>Subcomando: Set-Server

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/Authorize: {Sim &#124; não}]|Especifica se é para autorizar este servidor no protocolo DHCP.|
|[/RogueDetection: {Sim &#124; não}]|Habilita ou desabilita a detecção não autorizada de DHCP.|
|[/AnswerClients: {todos os &#124; conhecidos &#124; nenhum}]|Especifica quais clientes este servidor responderá. Se você definir esse valor como **conhecido**, um computador deverá ser pré-configurado nos serviços de domínio Active directory (AD DS) antes que ele seja respondido pelo servidor dos serviços de implantação do Windows.|
|[/ResponseDelay: <time in seconds> ]|A quantidade de tempo que o servidor aguardará antes de responder a um cliente de inicialização. Essa configuração não se aplica a computadores pré-configurados.|
|[/AllowN12forNewClients: {Sim &#124; não}]|para o Windows Server 2008, especifica que os clientes desconhecidos não terão que pressionar a tecla F12 para iniciar uma inicialização de rede. Os clientes conhecidos receberão o programa de inicialização especificado para o computador ou, se não for especificado, o programa de inicialização especificado para a arquitetura.<p>para o Windows Server 2008 R2, essa opção foi substituída pelo seguinte comando: WDSUTIL/Set-Server/PxepromptPolicy/New: noprompt|
|[/ArchitectureDiscovery: {Sim &#124; não}]|Habilita ou desabilita a descoberta de arquitetura. Isso facilita a descoberta de clientes baseados em x64 que não difundem sua arquitetura corretamente.|
|[/resetBootProgram: {Sim &#124; não}]|Determina se o caminho de inicialização será apagado para um cliente que acabou de ser inicializado sem a necessidade de um pressionamento de tecla F12.|
|[/DefaultX86X64Imagetype: {x86 &#124; x64 &#124;}]|Controla quais imagens de inicialização serão mostradas para clientes baseados em x64.|
|[/UseDhcpPorts: {Sim &#124; não}]|Especifica se o servidor PXE deve tentar se associar à porta DHCP, porta TCP 67. Se os serviços de implantação do DHCP e do Windows estiverem em execução no mesmo computador, você deverá definir essa opção como **não** para habilitar o servidor DHCP para utilizar a porta e definir o parâmetro **/DhcpOption60** como **Sim**. A configuração padrão para esse valor é **Sim**.|
|[/DhcpOption60: {Sim &#124; não}]|Especifica se a opção de DHCP 60 deve ser configurada para suporte a PXE. Se o DHCP e os serviços de implantação do Windows estiverem em execução no mesmo servidor, defina essa opção como **Sim** e defina a opção **/UseDhcpPorts** como **não**. A configuração padrão para esse valor é **não**.|
|[/RpcPort: <Port number> ]|Especifica o número da porta TCP a ser usado para atender às solicitações do cliente.|
|[/PxepromptPolicy]|Configura o quão conhecido (pré-configurado) e novos clientes iniciam uma inicialização PXE. Essa opção se aplica somente ao Windows Server 2008 R2. Defina as configurações usando as seguintes opções:<p>-[/Known: {Optin&#124;OptOut&#124;noprompt}] – define a política para clientes pré-configurados.<br />-[/New: {Optin&#124;optou de&#124;noprompt}] – define a política para novos clientes.<p>**Optin** significa que o cliente precisa pressionar uma tecla para inicializar o PXE, caso contrário, ele retornará ao próximo dispositivo de inicialização.<p>**Noprompt** significa que o cliente sempre será inicializado por PXE.<p>**Optour** significa que o cliente será inicializado PXE, a menos que a tecla ESC seja pressionada.|
|[/BootProgram: <Relative path> ] /Architecture: {x86 &#124; IA64 &#124; x64}|Especifica o caminho relativo para o programa de inicialização na pasta remoteInstall (por exemplo, **boot\x86\pxeboot.n12**) e especifica a arquitetura do programa de inicialização.|
|[/N12BootProgram: <Relative path> ] /Architecture: {x86 &#124; IA64 &#124; x64}|Especifica o caminho relativo para o programa de inicialização que não exige o pressionamento da tecla F12 (por exemplo, **boot\x86\pxeboot.n12**) e especifica a arquitetura do programa de inicialização.|
|[/BootImage: <Relative path> ] /Architecture: {x86 &#124; IA64 &#124; x64}|Especifica o caminho relativo para a imagem de inicialização que os clientes de inicialização devem receber e especifica a arquitetura da imagem de inicialização. Você pode especificar isso para cada arquitetura.|
|[/PreferredDC: <DC Name> ]|Especifica o nome do controlador de domínio que os serviços de implantação do Windows devem usar. Pode ser o nome NetBIOS ou o FQDN.|
|[/PreferredGC: <GC Name> ]|Especifica o nome do servidor de catálogo global que os serviços de implantação do Windows devem usar. Pode ser o nome NetBIOS ou o FQDN.|
|[/PrestageUsingMAC: {Sim &#124; não}]|Especifica se os serviços de implantação do Windows, ao criar contas de computador no AD DS, devem usar o endereço MAC em vez do GUID/UUID para identificar o computador.|
|[/NewMachineNamingPolicy: <Policy> ]|Especifica o formato a ser usado ao gerar nomes de computador para clientes. Para obter informações sobre o formato a ser usado para o <policy> , clique com o botão direito do mouse no servidor no snap-in do MMC, clique em **Propriedades**e exiba a guia **serviços de diretório** . Por exemplo, **/NewMachineNamingPolicy:% 61Username% #**.|
|[/NewMachineOU]|Usado para especificar o local em AD DS em que as contas de computador cliente serão criadas. Especifique o local usando as opções a seguir.<p>-[/Type: ServerDomain &#124; UserDomain &#124; o UserOU &#124; Custom] Especifica o tipo de local. O **ServerDomain** cria contas no mesmo domínio que o servidor dos serviços de implantação do Windows. **UserDomain** cria contas no mesmo domínio que o usuário que está executando a instalação. **Userou** cria contas na unidade organizacional do usuário que está executando a instalação. **Personalizado** permite que você especifique um local personalizado (você também deve especificar um valor para **/ou** com essa opção).<br />-[/OU: <Domain name of OU> ]-se você especificar **personalizado** para a opção **/Type** , essa opção especificará a unidade organizacional em que as contas de computador devem ser criadas.|
|[/DomainSearchOrder: {GCOnly &#124; DCFirst}]|Especifica a política para pesquisar contas de computador no AD DS (catálogo global ou controlador de domínio).|
|[/NewMachineDomainJoin: {Sim &#124; não}]|Especifica se um computador que ainda não está pré-configurado no AD DS deve ser ingressado no domínio durante a instalação. A configuração padrão é **Sim**.|
|[/WdsClientLogging]|Especifica o nível de log para o servidor.<p>-[/Enabled: {Yes &#124; no}] – habilita ou desabilita o registro em log de ações do cliente dos serviços de implantação do Windows.<br />-[/LoggingLevel: {None &#124; erros &#124; avisos &#124; info} – define o nível de log. **None** é equivalente a desabilitar o registro em log. **Erros** é o nível mais baixo de registro em log e indica que apenas os erros serão registrados. Os **avisos** incluem avisos e erros. **Info** é o nível mais alto de registro em log e inclui erros, avisos e eventos informativos.|
|[/WdsUnattend]|Essas configurações controlam o comportamento de instalação autônoma do cliente dos serviços de implantação do Windows. Defina as configurações usando as seguintes opções:<p>-[/Policy: {Enabled &#124; disabled}]-Especifica se a instalação autônoma é usada ou não.<br />-[/CommandlinePrecedence: {Yes &#124; no}] – Especifica se um arquivo de Autounattend.xml (se presente no cliente) ou um arquivo de instalação autônoma que foi passado diretamente para o cliente dos serviços de implantação do Windows com a opção/unattend será usado em vez de um arquivo autônomo de imagem durante uma instalação do cliente. A configuração padrão é **não**.<br />-[/File: <Relative path> /Architecture: {x86 &#124; ia64 &#124; x64}] – Especifica o nome do arquivo, o caminho e a arquitetura do arquivo autônomo.|
|[/AutoaddPolicy]|Essas configurações controlam a política de adição automática. Você define as configurações usando as seguintes opções:<p>-[/Policy: {AdminApproval &#124; disabled}]- **AdminApprove** faz com que todos os computadores desconhecidos sejam adicionados a uma fila pendente, em que o administrador pode examinar a lista de computadores e aprovar ou rejeitar cada solicitação, conforme apropriado. **Disabled** indica que nenhuma ação adicional é executada quando um computador desconhecido tenta inicializar no servidor.<br />-[/PollInterval: {tempo em segundos}] – Especifica o intervalo (em segundos) no qual o programa de inicialização de rede deve sondar o servidor dos serviços de implantação do Windows.<br />-[/MaxRetry: <Number> ] – Especifica o número de vezes que o programa de inicialização de rede deve sondar o servidor dos serviços de implantação do Windows. Esse valor, junto com **/PollInterval**, determina por quanto tempo o programa de inicialização de rede aguardará que um administrador aprove ou rejeite o computador antes de atingir o tempo limite. Por exemplo, um valor de **MaxRetry** de 10 e **intervalopesquisa** vlue de 60 indicaria que o cliente deve sondar o servidor 10 vezes, aguardando 60 segundos entre as tentativas. Portanto, o cliente atingiria o tempo limite após 10 minutos (10 x 60 segundos = 10 minutos).<br />-[/Message: <Message> ] – Especifica a mensagem que é exibida para o cliente na página da caixa de diálogo do programa de inicialização de rede.<br />-[/RetentionPeriod]-Especifica o número de dias que um computador pode estar em um estado pendente antes de ser limpo automaticamente.<br />-[/Approved: <time in days> ]-Especifica o período de retenção para computadores aprovados. Você deve usar esse parâmetro com a opção **/RetentionPeriod** .<br />-[/Others: <time in days> ]-Especifica o período de retenção para computadores não aprovados (rejeitados ou pendentes). Você deve usar esse parâmetro com a opção **/RetentionPeriod** .|
|[/AutoaddSettings]|Especifica as configurações padrão a serem aplicadas a cada computador. Você define as configurações usando as seguintes opções:<p>-/Architecture: {x86 &#124; IA64 &#124; x64} – especifica a arquitetura.<br />-[/BootProgram: <Relative path> ] – Especifica o programa de inicialização enviado para o computador aprovado. Se nenhum programa de inicialização for especificado, o padrão para a arquitetura do computador (conforme especificado no servidor) será usado.<br />-[/WdsClientUnattend: <Relative path> ] – define o caminho relativo para o arquivo autônomo que o cliente aprovado deve receber.<br />-[/ReferralServer: <Server name> ]-Especifica o servidor de serviços de implantação do Windows que o cliente usará para baixar imagens.<br />-[/BootImage: <Relative path> ]-Especifica a imagem de inicialização que o cliente aprovado receberá.<br />-[/User: <domínio \ usuário &#124; User@Domain>] – define permissões no objeto de conta de computador para dar ao usuário especificado os direitos necessários para ingressar o computador no domínio.<br />-[JoinRights: {JoinOnly &#124; Full}]-Especifica o tipo de direitos a serem atribuídos ao usuário. O **JoinOnly** exige que o administrador redefina a conta de computador antes que o usuário possa ingressar o computador no domínio. **Completo** dá acesso completo ao usuário, incluindo o direito de ingressar o computador no domínio.<br />-[/JoinDomain: {Yes &#124; no}] – Especifica se o computador deve ser ingressado no domínio como esta conta de computador durante uma instalação dos serviços de implantação do Windows. A configuração padrão é **Sim**.|
|[/BindPolicy]|Configura as interfaces de rede para o provedor PXE escutar. Você define a política usando as seguintes opções:<p>-[/Policy: {include &#124; Exclude}] – define a política de associação de interface para incluir ou excluir os endereços na lista de interfaces.<br />-[/Add] – adiciona uma interface à lista. Você também deve especificar/AddressType e/address.<br />-[/remove] – remove uma interface da lista. Você também deve especificar/AddressType e/address.<br />-/Address: <IP or MAC address> -especifica o endereço IP ou Mac da interface a ser adicionada ou removida.<br />-/AddressType: {IP &#124; MAC}-indica o tipo de endereço especificado na opção **/Address** .|
|[/RefreshPeriod: <seconds> ]|Especifica com que frequência (em segundos) o servidor atualizará suas configurações.|
|[/BannedGuidPolicy]|Gerencia a lista de GUIDs banidos usando as seguintes opções:<p>-[/Add]/GUID: <GUID> -adiciona o GUID especificado à lista de GUIDs banidos. Em vez disso, qualquer cliente com esse GUID será identificado pelo seu endereço MAC.<br />-[/remove]/GUID: <GUID> -Remove o GUID especificado da lista de GUIDs banidos.|
|[/BcdRefreshPolicy]|Define as configurações para atualizar os arquivos BCD usando as seguintes opções:<p>-[/Enabled: {Yes &#124; no}]-Especifica a política de atualização de BCD. Quando **/Enabled** é definido como **Sim**, os arquivos BCD são atualizados no intervalo de tempo especificado.<br />-[/RefreshPeriod: <time in minutes> ] – Especifica o intervalo de tempo no qual os arquivos BCD são atualizados.|
|[/Transport]|Configura as seguintes opções:<p><ul><li>[/ObtainIpv4From: {DHCP &#124; Range}] – Especifica a origem dos endereços IPv4.<p><ul><li>[/start: <starting Ipv4 address> ] -Especifica o início do intervalo de endereços IP. Essa opção é necessária e válida somente se **/ObtainIpv4From** estiver definido como **intervalo**</li><li>[/End: <Ending Ipv4 address> ] -Especifica o final do intervalo de endereços IP. Essa opção é necessária e válida somente se **/ObtainIpv4From** estiver definido como **Range**.</li></ul></li><li>[/ObtainIpv6From: intervalo] [/start: <start IP address> ] [/End: <End IP address> ]  Especifica a origem dos endereços IPv6. Essa opção se aplica somente ao Windows Server 2008 R2 e o único valor com suporte é Range.</li><li>[/startPort: <starting port> ] -Especifica o início do intervalo de portas.</li><li>[/EndPort: <Ending port> ] -Especifica o fim do intervalo de portas.</li><li>[/Profile: {10 Mbps &#124; 100 Mbps &#124; 1 Gbps &#124; Custom}]-Especifica o perfil de rede a ser usado. Essa opção só tem suporte forservers executando o Windows Server 2008.</li><li>[/MulticastSessionPolicy]  Define as configurações de transferência para transmissões multicast. Este comando só está disponível para o Windows Server 2008 R2.<p><ul><li>[/Policy: {None &#124; a desconexão automática &#124; MultiStream}] – determina como lidar com clientes lentos. Nenhum significa manter todos os clientes em uma sessão na mesma velocidade. A desconexão automática significa que todos os clientes que estiverem abaixo do/Threshold especificado serão desconectados. MultiStream significa que os clientes serão separados em várias sessões, conforme especificado por/StreamCount.</li><li>[/Threshold: <Speed in KBps> ] -para/Policy: Autodisconnect, essa opção define a taxa de transferência mínima em KBps. Os clientes que se descartarem abaixo dessa taxa serão desconectados das transmissões multicast.</li><li>[/StreamCount: {2 &#124; 3}] [/Fallback: {Yes &#124; no}]-para/Policy: MultiStream, essa opção determina o número de sessões. 2 significa duas sessões (rápidas e lentas) 3 significa três sessões (lenta, média, rápida).</li><li>[/Fallback: {Yes&#124; no}] – determina se os clientes desconectados continuarão a transferência usando outro método (se houver suporte do cliente). Se você estiver usando o cliente WDS, o computador fará fallback para unicasting. Wdsmcast.exe não dá suporte a um mecanismo de fallback. Essa opção também se aplica a clientes que não dão suporte a MultiStream. Nesse caso, o computador voltará para outro método em vez de mover para uma sessão de transferência mais lenta.</li></ul></li></ul>|
## <a name="examples"></a>Exemplos
Para definir o servidor para responder somente a clientes conhecidos, com um atraso de resposta de 4 minutos, digite:
```
wdsutil /Set-Server /AnswerClients:Known /Responsedelay:4
```
Para definir o programa de inicialização e a arquitetura do servidor, digite:
```
wdsutil /Set-Server /BootProgram:boot\x86\pxeboot.n12 /Architecture:x86
```
Para habilitar o registro em log no servidor, digite:
```
wdsutil /Set-Server /WdsClientLogging /Enabled:Yes /LoggingLevel:Warnings
```
Para habilitar o autônomo no servidor, bem como a arquitetura e o arquivo autônomo do cliente, digite:
```
wdsutil /Set-Server /WdsUnattend /Policy:Enabled /File:WDSClientUnattend \unattend.xml /Architecture:x86
```
Para definir o servidor PXE (Pre-boot Execution Environment) para tentar se associar às portas TCP 67 e 60, digite:
```
wdsutil /Set-server /UseDhcpPorts:No /DhcpOption60:Yes
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-disable-server-command.md) 
 Disable-Server [Usando o comando](using-the-enable-server-command.md) 
 Enable-Server [Usando o comando](using-the-get-server-command.md) 
 Get-Server [Usando o comando](using-the-initialize-server-command.md) 
 Initialize-Server [Subcomando: Start-Server](subcommand-start-server.md) 
 [Subcomando: Stop-Server](subcommand-stop-server.md) 
 [A opção Uninitialize-Server](the-uninitialize-server-option.md)
