---
title: Visão geral do DNS anycast
description: Este tópico fornece uma breve visão geral do DNS anycast
manager: laurawi
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: greglin
author: greg-lindsay
ms.openlocfilehash: f43c1978e193cbb2212966ab519b002d9fed45f5
ms.sourcegitcommit: ccd38245f1b766be005d0c257962f756ff0c4e76
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92175801"
---
# <a name="anycast-dns-overview"></a>Visão geral do DNS anycast

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2019

Este tópico fornece informações sobre como o DNS anycast funciona.

## <a name="what-is-anycast"></a>O que é anycast?

A anycast é uma tecnologia que fornece vários caminhos de roteamento para um grupo de pontos de extremidade que são atribuídos ao mesmo endereço IP. Cada dispositivo do grupo anuncia o mesmo endereço em uma rede e os protocolos de roteamento são usados para escolher qual é o melhor destino.

A anycast permite que você dimensione um serviço sem estado, como DNS ou HTTP, colocando vários nós atrás do mesmo endereço IP e usando o roteamento de ECMP (vários caminhos de custo igual) para direcionar o tráfego entre esses nós. A anycast é diferente da unicast, na qual cada ponto de extremidade tem seu próprio endereço IP separado. 

## <a name="why-use-anycast-with-dns"></a>Por que usar anycast com DNS?

Com o DNS anycast, você pode habilitar um servidor DNS ou um grupo de servidores para responder a consultas DNS com base na localização geográfica de um cliente DNS. Isso pode melhorar o tempo de resposta DNS e simplificar as configurações do cliente DNS. O DNS anycast também fornece uma camada extra de redundância e pode ajudar a proteger contra ataques de negação de serviço DNS. 

### <a name="how-anycast-dns-works"></a>Como funciona o DNS anycast

O DNS anycast funciona usando protocolos de roteamento como Border Gateway Protocol (BGP) para enviar consultas DNS a um servidor DNS preferencial ou grupo de servidores DNS (por exemplo: um grupo de servidores DNS gerenciados por um balanceador de carga). Isso pode otimizar as comunicações DNS obtendo respostas DNS de um servidor DNS mais próximo de um cliente.

Com a anycast, os servidores que existem em várias localizações geográficas anunciam um único endereço IP idêntico para seu gateway local (roteador). Quando um cliente DNS inicia uma consulta para o endereço anycast, as rotas disponíveis são avaliadas e a consulta DNS é enviada para o local preferencial. Em geral, esse é o local mais próximo com base na topologia de rede. Veja o exemplo a seguir.

![DNS Anycast](../../media/Anycast/anycast.png)

**Figura 1**: quatro servidores DNS localizados em sites diferentes em uma rede cada um anuncia o mesmo endereço IP de anycast (setas pretas) para a rede. Um dispositivo cliente DNS envia uma solicitação para o endereço IP de anycast. Os dispositivos de rede analisam as rotas disponíveis e enviam a consulta DNS do cliente para o local mais próximo (seta azul). 

O DNS anycast é usado normalmente hoje para rotear o tráfego DNS para muitos serviços DNS globais. Por exemplo, o sistema de servidor DNS raiz depende muito do DNS anycast. A anycast também funciona com uma variedade de protocolos de roteamento e pode ser usada exclusivamente em intranets.

## <a name="windows-server-native-bgp-anycast-demo"></a>Demonstração de anycast do BGP nativo do Windows Server

O procedimento a seguir demonstra como o BGP nativo no Windows Server pode ser usado com o DNS anycast.  

### <a name="requirements"></a>Requisitos

- Um dispositivo físico com a função Hyper-V instalada.
  - Windows Server 2012 R2, Windows 10 ou posterior.
- 2 VMs de cliente (qualquer sistema operacional).
  - A instalação de ferramentas de ligação para DNS, como a busca, é recomendada.
- 3 VMs de servidor (Windows Server 2016 ou Windows Server 2019).
  - Se o módulo LoopbackAdapter do Windows PowerShell ainda não estiver instalado em VMs de servidor (DC001, DC002), o acesso à Internet será temporariamente necessário para instalar esse módulo.

### <a name="hyper-v-setup"></a>Instalação do Hyper-V

Configure o servidor Hyper-V da seguinte maneira:

- 2 redes privadas do comutador virtual estão configuradas
  - Uma rede de Internet fictícia 131.253.1.0/24
  - Uma rede de intranet fictícia rede 10.10.10.0/24
- 2 VMs de cliente são anexadas à rede 131.253.1.0/24
- 2 as VMs de servidor estão conectadas à rede rede 10.10.10.0/24
- 1 o servidor é de hospedagem dupla e é anexado às redes 131.253.1.0/24 e rede 10.10.10.0/24.

### <a name="virtual-machine-network-configuration"></a>Configuração de rede da máquina virtual

Defina as configurações de rede em máquinas virtuais com as seguintes configurações:

1.  CLIENT1, CLIENT2
  - CLIENT1:131.253.1.1
  - Client2:131.253.1.2
  - Máscara de sub-rede: 255.255.255.0
  - DNS: 51.51.51.51
  - Gateway: 131.253.1.254
2.  Gateway (Windows Server)
  - NIC1:131.253.1.254, sub-rede 255.255.255.0
  - NIC2:10.10.10.254, sub-rede 255.255.255.0
  - DNS: 51.51.51.51
  - Gateway: 131.253.1.100 (pode ser ignorado para a demonstração)
3.  DC001 (Windows Server)
  - NIC1:10.10.10.1
  - Sub-rede: 255.255.255.0
  - DNS: 10.10.10.1
  - Gateway: 10.10.10.254
4.  DC002 (Windows Server)
  - NIC1:10.10.10.2
  - Sub-rede 255.255.255.0
  - DNS: 10.10.10.2 *
  - Gateway: 10.10.10.254

   * Use 10.10.10.1 para DNS inicialmente ao executar o ingresso no domínio para DC002 para que você possa localizar o domínio Active Directory no DC001.

### <a name="configure-dns"></a>Configurar DNS

Use Gerenciador do Servidor e o console de gerenciamento do DNS ou o Windows PowerShell para instalar as seguintes funções de servidor e criar uma zona DNS estática em cada um dos dois servidores.

1.  DC001, DC002
  - Instalar Active Directory Domain Services e promover para o controlador de domínio (opcional)
  - Instalar a função DNS (obrigatório)
  - Crie uma zona estática (não integrada ao AD) chamada **Zone. TST** em DC001 e DC002
    - Adicionar o **servidor** de nome de registro estático único na zona do tipo "txt"
    - Dados (texto) para o registro TXT em DC001 = **DC001**
    - Dados (texto) para o registro TXT em DC002 = **DC002**

### <a name="configure-loopback-adapters"></a>Configurar adaptadores de loopback

Insira os seguintes comandos em um prompt elevado do Windows PowerShell em DC001 e DC002 para configurar adaptadores de loopback. 

> [!NOTE]
> O comando **install-Module** requer acesso à Internet. Isso pode ser feito por meio da atribuição temporária da VM a uma rede externa no Hyper-V.

```PowerShell
$primary_interface = (Get-NetAdapter |?{$_.Status -eq "Up" -and !$_.Virtual}).Name
$loopback_ipv4 = '51.51.51.51'
$loopback_ipv4_length = '32'
$loopback_name = 'Loopback'
Install-Module -Name LoopbackAdapter -MinimumVersion 1.2.0.0 -Force
Import-Module -Name LoopbackAdapter
New-LoopbackAdapter -Name $loopback_name -Force
$interface_loopback = Get-NetAdapter -Name $loopback_name
$interface_main = Get-NetAdapter -Name $primary_interface
Set-NetIPInterface -InterfaceIndex $interface_loopback.ifIndex -InterfaceMetric "254" -WeakHostReceive Enabled -WeakHostSend Enabled -DHCP Disabled
Set-NetIPInterface -InterfaceIndex $interface_main.ifIndex -WeakHostReceive Enabled -WeakHostSend Enabled
Set-NetIPAddress -InterfaceIndex $interface_loopback.ifIndex -SkipAsSource $True
Get-NetAdapter $loopback_name | Set-DNSClient –RegisterThisConnectionsAddress $False
New-NetIPAddress -InterfaceAlias $loopback_name -IPAddress $loopback_ipv4 -PrefixLength $loopback_ipv4_length -AddressFamily ipv4
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_msclient
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_pacer
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_server
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_lltdio
Disable-NetAdapterBinding -Name $loopback_name -ComponentID ms_rspndr
```

### <a name="virtual-machine-routing-configuration"></a>Configuração de roteamento de máquina virtual

Use os seguintes comandos do Windows PowerShell em VMs para configurar o roteamento.

1.  Gateway
```PowerShell
Install-WindowsFeature RemoteAccess -IncludeManagementTools
Install-RemoteAccess -VpnType RoutingOnly
Add-BgpRouter -BgpIdentifier “10.10.10.254” -LocalASN 8075
Add-BgpPeer -Name "DC001" -LocalIPAddress 10.10.10.254 -PeerIPAddress 10.10.10.1 -PeerASN 65511 –LocalASN 8075
```

2.  DC001
```PowerShell
Install-WindowsFeature RemoteAccess -IncludeManagementTools
Install-RemoteAccess -VpnType RoutingOnly
Add-BgpRouter -BgpIdentifier “10.10.10.1” -LocalASN 65511
Add-BgpPeer -Name "Labgw" -LocalIPAddress 10.10.10.1 -PeerIPAddress 10.10.10.254 -PeerASN 8075 –LocalASN 65511
Add-BgpCustomRoute -Network 51.51.51.0/24
```

3.  DC002
```PowerShell
Install-WindowsFeature RemoteAccess -IncludeManagementTools
Install-RemoteAccess -VpnType RoutingOnly
Add-BgpRouter -BgpIdentifier "10.10.10.2" -LocalASN 65511
Add-BgpPeer -Name "Labgw" -LocalIPAddress 10.10.10.2 -PeerIPAddress 10.10.10.254 -PeerASN 8075 –LocalASN 65511
Add-BgpCustomRoute -Network 51.51.51.0/24
```

### <a name="summary-diagram"></a>Diagrama de resumo

![DNS Anycast](../../media/Anycast/anycast-lab.png)

**Figura 2**: configuração de laboratório para demonstração de DNS de anycast BGP nativo

## <a name="anycast-dns-demonstration"></a>Demonstração de DNS anycast


1.  Verificar o roteamento BGP no servidor de gateway

    PS C: \> Get-BgpRouteInformation

    DestinationNetwork NextHop LearnedFromPeer State LocalPref MED<br>
    ------------------ -------    --------------- ----- --------- ---<br>
    51.51.51.0/24 10.10.10.1 DC001 melhor<br>
    51.51.51.0/24 10.10.10.2 DC002 melhor<br>

2.  Em CLIENT1 e CLIENT2, verifique se você pode alcançar o 51.51.51.51

    PS C: \> ping 51.51.51.51

    Pinging 51.51.51.51 com 32 bytes de dados:<br>
    Resposta de 51.51.51.51: bytes = 32 tempo<1 MS TTL = 126<br>
    Resposta de 51.51.51.51: bytes = 32 tempo<1 MS TTL = 126<br>
    Resposta de 51.51.51.51: bytes = 32 tempo<1 MS TTL = 126<br>
    Resposta de 51.51.51.51: bytes = 32 tempo<1 MS TTL = 126

    Estatísticas de ping para 51.51.51.51:<br>
    Pacotes: enviados = 4, recebidos = 4, perdidos = 0 (0% de perda),<br>
    Tempos de ida e volta aproximados em mili-segundos:<br>
    Mínimo = 0ms, máximo = 0ms, média = 0ms

    > [!NOTE]
    > Se o ping falhar, verifique também se não há nenhuma regra de firewall bloqueando o ICMP.

3.  Em CLIENT1 e CLIENT2, use nslookup ou mergulhe para consultar o registro TXT. Exemplos de ambos são mostrados abaixo.

    PS C: \> mergulhe Server. Zone. TST txt + Short<br>
    PS C: \> nslookup-Type = txt servidor. Zone. TST 51.51.51.51

    Um cliente exibirá "DC001" e o outro cliente exibirá "DC002" verificando se o anycast está funcionando corretamente.  Você também pode consultar no servidor de gateway.

4.  Em seguida, desabilite o adaptador Ethernet em DC001.

    PS C: \> (Get-netadapter). Nomes<br>
    Loopback<br>
    Ethernet 2<br>
    PS C: \> Disable-NetAdapter "Ethernet 2"<br>
    Confirmar<br>
    Tem certeza de que deseja executar esta ação?<br>
    Disable-NetAdapter ' Ethernet 2 '<br>
    [Y] Sim [A] Sim para todos [N] não [L] não para todos [S] suspender [?] Ajuda (o padrão é "Y"):<br>
    PS C: \> (Get-netadapter). Estado<br>
    Up<br>
    Desabilitado

5.  Confirme se os clientes DNS que estavam recebendo respostas anteriormente de DC001 mudaram para DC002.

    PS C: \> nslookup-Type = txt servidor. Zone. TST 51.51.51.51<br>
    Servidor: desconhecido<br>
    Endereço: 51.51.51.51<br>

    servidor. Zone. TST Text =

    "DC001"<br>
    PS C: \> nslookup-Type = txt servidor. Zone. TST 51.51.51.51<br>
    Servidor: desconhecido<br>
    Endereço: 51.51.51.51<br>

    servidor. Zone. TST Text =

    "DC002"

6.  Confirme se a sessão BGP está inoperante no DC001 usando Get-BgpStatistics no servidor de gateway.
7.  Habilite o adaptador Ethernet em DC001 novamente e confirme se a sessão BGP foi restaurada e se os clientes recebem respostas DNS de DC001 novamente.

> [!NOTE]
> Se um balanceador de carga não for usado, um cliente individual usará o mesmo servidor DNS de back-end, se estiver disponível. Isso cria um caminho BGP consistente para o cliente. Para obter mais informações, consulte a seção 4.4.3 de RFC4786: [caminhos de custo igual](https://tools.ietf.org/html/rfc4786#page-10).

## <a name="frequently-asked-questions"></a>Perguntas frequentes

P: o DNS de anycast é uma boa solução para usar em um ambiente DNS local?<br>
R: o DNS de anycast funciona diretamente com um serviço DNS local. No entanto, a anycast não é *necessária* para que o serviço DNS seja dimensionado.
 
P: Qual é o impacto da implementação do DNS anycast em um ambiente com um número grande (ex: >50) de controladores de domínio? <br>
R: não há nenhum impacto direto na funcionalidade. Se um balanceador de carga for usado, nenhuma configuração adicional nos controladores de domínio será necessária.
 
P: uma configuração de DNS anycast com suporte do atendimento ao cliente da Microsoft?<br>
R: se você usar um balanceador de carga não Microsoft para encaminhar consultas DNS, a Microsoft dará suporte a problemas relacionados ao serviço do servidor DNS. Consulte o fornecedor do balanceador de carga para problemas relacionados ao encaminhamento de DNS. 
 
P: Qual é a melhor prática para o DNS de anycast com um número grande (ex: >50) de controladores de domínio?<br>
R: a prática recomendada é usar um balanceador de carga em cada local geográfico. Os balanceadores de carga normalmente são fornecidos por um fornecedor externo. 

P: o DNS de anycast e o DNS do Azure têm funcionalidade semelhante?<br>
R: o DNS do Azure usa Anycast. Para usar a anycast com o DNS do Azure, configure o balanceador de carga para encaminhar solicitações para o servidor DNS do Azure. 