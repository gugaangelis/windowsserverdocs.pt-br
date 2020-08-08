---
title: Balanceamento de carga do software (SLB) para SDN
description: Você pode usar este tópico para saber mais sobre o balanceamento de carga de software para a rede definida pelo software no Windows Server 2016.
manager: grcusanz
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 43591a1cca143037e9abe555321276cb0f83263b
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995506"
---
# <a name="software-load-balancing-slb-for-sdn"></a>\(SLB \) de balanceamento de carga de software para Sdn

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre o balanceamento de carga de software para a rede definida pelo software no Windows Server 2016.

Os CSPs (provedores de serviços de nuvem) e as empresas que estão implantando SDN (rede definida pelo software) no Windows Server 2016 podem usar o SLB (balanceamento de carga de software) para distribuir uniformemente o tráfego de rede do locatário e do cliente de locatário entre os recursos de rede virtual. O SLB do Windows Server permite que vários servidores hospedem a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade.

O SLB do Windows Server inclui os seguintes recursos.

-   Serviços de balanceamento de carga de camada 4 (L4) para o tráfego de TCP/UDP "Norte-Sul" e "leste-oeste".

-   Balanceamento de carga de tráfego de rede interno e público.

-   O dá suporte a DIPs (endereços IP dinâmicos) em redes locais virtuais (VLANs) e em redes virtuais que você cria usando a virtualização de rede Hyper-V.

-   Suporte à investigação de integridade.

-   Pronto para a escala de nuvem, incluindo capacidade de expansão e capacidade de expansão para multiplexadores e agentes de host.

Para obter mais informações, consulte [recursos de balanceamento de carga de software](#bkmk_features) neste tópico.

> [!NOTE]
> O controlador de rede não dá suporte a multilocação para VLANs. no entanto, você pode usar VLANs com SLB para cargas de trabalho gerenciadas do provedor de serviços, como a infraestrutura do datacenter e servidores Web de alta densidade.

Usando o SLB do Windows Server, você pode escalar horizontalmente seus recursos de balanceamento de carga usando VMs SLB nos mesmos servidores de computação do Hyper-V que você usa para suas outras cargas de trabalho de VM. Por isso, o SLB dá suporte à criação e exclusão rápida de pontos de extremidade de balanceamento de carga que são necessários para operações de CSP. Além disso, o SLB do Windows Server dá suporte a dezenas de gigabytes por cluster, fornece um modelo de provisionamento simples e é fácil de expandir e reduzir.

**Como o SLB funciona**

O SLB funciona mapeando VIPs (endereços IP virtuais) para DIPs (endereços IP dinâmicos) que fazem parte de um conjunto de recursos de serviço de nuvem no datacenter.

VIPs são endereços IP únicos que fornecem acesso público a um pool de VMs com balanceamento de carga. Por exemplo, VIPs são endereços IP expostos na Internet para que os locatários e clientes locatários possam se conectar a recursos de locatário no datacenter na nuvem.

DIPs são os endereços IP das VMs membro de um pool com balanceamento de carga por trás do VIP. Os DIPs são atribuídos dentro da infraestrutura de nuvem para os recursos de locatário.

VIPs estão localizados no multiplexador SLB (MUX).  O MUX consiste em uma ou mais VMs (máquinas virtuais).  O controlador de rede fornece a cada MUX cada VIP, e cada MUX, por sua vez, usa Border Gateway Protocol (BGP) para anunciar cada VIP aos roteadores na rede física como uma rota/32.  O BGP permite que os roteadores de rede física:

-   Saiba que um VIP está disponível em cada MUX, mesmo que os MUXes estejam em sub-redes diferentes em uma rede de camada 3.

-   Espalhe a carga para cada VIP em todos os MUXes disponíveis usando o roteamento de vários caminhos (ECMP) de custo igual.

-   Detectar automaticamente uma falha ou remoção de MUX e parar de enviar tráfego para o MUX com falha.

-   Espalhe a carga do MUX com falha ou removido entre os MUXes íntegros.

Quando o tráfego público chega da Internet, o SLB MUX examina o tráfego, que contém o VIP como um destino, e mapeia e reescreve o tráfego para que ele chegue a um DIP individual. Para o tráfego de rede de entrada, essa transação é executada em um processo de duas etapas que é dividido entre as VMs (máquinas virtuais) do MUX e o host do Hyper-V onde o DIP de destino está localizado:

-   Balanceamento de carga – a MUX usa o VIP para selecionar um DIP, encapsula o pacote e encaminha o tráfego para o host do Hyper-V onde o DIP está localizado.

-   NAT (conversão de endereços de rede) – o host Hyper-V remove o encapsulamento do pacote, converte o VIP em um DIP, remapeia as portas e encaminha o pacote para a VM DIP.

O MUX sabe como mapear VIPs para o DIPs correto devido às políticas de balanceamento de carga que você define usando o controlador de rede. Essas regras incluem protocolo, porta de front-end, porta de back-end e algoritmo de distribuição (5, 3 ou 2 tuplas).

Quando as VMs de locatário respondem e enviam o tráfego de rede de saída para a Internet ou para locais de locatário remotos, como o NAT é executado pelo host Hyper-V, o tráfego ignora o MUX e vai diretamente para o roteador de borda do host Hyper-V. Esse processo de bypass de MUX é chamado de retorno de servidor direto (DSR).

E, depois que o fluxo de tráfego de rede inicial é estabelecido, o tráfego de rede de entrada ignora completamente o SLB MUX.

Na ilustração a seguir, um computador cliente executa uma consulta DNS para o endereço IP de um site da empresa do SharePoint – nesse caso, uma empresa fictícia chamada contoso. O processo a seguir ocorre.

-   O servidor DNS retorna o 107.105.47.60 VIP para o cliente.

-   O cliente envia uma solicitação HTTP para o VIP.

-   A rede física tem vários caminhos disponíveis para alcançar o VIP localizado em qualquer MUX.  Cada roteador ao longo do caminho usa ECMP para escolher o próximo segmento do caminho até chegar à solicitação em um MUX.

-   O MUX que recebe a solicitação verifica as políticas configuradas e vê que há dois DIPs disponíveis, 10.10.10.5 e 10.10.20.5, em uma rede virtual para lidar com a solicitação para o VIP 107.105.47.60

-   O MUX seleciona o DIP 10.10.10.5 e encapsula os pacotes usando VXLAN para que ele possa enviá-lo para o host que contém o DIP usando o endereço de rede física dos hosts.

-   O host recebe o pacote encapsulado e o inspeciona.  Ele remove o encapsulamento e reescreve o pacote, de modo que o destino agora é o DIP 10.10.10.5 em vez do VIP e envia o tráfego para a VM DIP.

-   A solicitação agora chegou ao site contoso SharePoint no farm de servidores 2. O servidor gera uma resposta e a envia para o cliente, usando seu próprio endereço IP como a origem.

-   O host intercepta o pacote de saída no comutador virtual que se lembra de que o cliente, agora, o destino, fez a solicitação original para o VIP.  O host reescreve a origem do pacote para ser o VIP para que o cliente não veja o endereço DIP.

-   O host encaminha o pacote diretamente para o gateway padrão para a rede física que usa sua tabela de roteamento padrão para encaminhar o pacote para o cliente que, eventualmente, recebe a resposta.

![Processo de balanceamento de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)

**Tráfego de datacenter interno de balanceamento de carga**

Ao balancear a carga do tráfego de rede interno ao datacenter, como entre os recursos de locatário que estão sendo executados em servidores diferentes e são membros da mesma rede virtual, o comutador virtual do Hyper-V ao qual as VMs estão conectadas executa o NAT.

Com o balanceamento de carga de tráfego interno, a primeira solicitação é enviada e processada pelo MUX, que seleciona o DIP apropriado e roteia o tráfego para o DIP. Desse ponto em diante, o fluxo de tráfego estabelecido ignora o MUX e vai diretamente da VM para a VM.

**Investigações de integridade**

O SLB inclui investigações de integridade para validar a integridade da infraestrutura de rede, incluindo as seguintes.

-   Investigação de TCP para porta

-   Investigação de HTTP para porta e URL

Ao contrário de um dispositivo de balanceador de carga tradicional em que a investigação se origina no dispositivo e viaja pela conexão com o DIP, a investigação de SLB se origina no host onde o DIP está localizado e vai diretamente do agente de host SLB para o DIP, distribuindo ainda mais o trabalho entre os hosts.

## <a name="software-load-balancing-infrastructure"></a><a name="bkmk_infrastructure"></a>Infraestrutura de balanceamento de carga de software
Para implantar o SLB do Windows Server, você deve primeiro implantar o controlador de rede no Windows Server 2016 e em uma ou mais VMs do SLB MUX.

Além disso, você deve configurar hosts Hyper-V com o comutador virtual Hyper-V habilitado para SDN e verificar se o agente de host SLB está em execução.  Os roteadores que atendem aos hosts devem oferecer suporte ao roteamento e Border Gateway Protocol (ECMP) de vários caminhos de custo igual e devem ser configurados para aceitar solicitações de emparelhamento BGP do SLB MUXes.

A seguir, uma visão geral da infraestrutura SLB.

![Infraestrutura de balanceamento de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)

As seções a seguir fornecem mais informações sobre esses elementos da infraestrutura SLB.

### <a name="scvmm"></a>SCVMM
Com o System Center 2016, você pode configurar o controlador de rede no Windows Server 2016, incluindo o Gerenciador de SLB e o Health Monitor. Você também pode usar o System Center para implantar o SLB MUXs e instalar agentes de host SLB em computadores que executam o Windows Server 2016 e o Hyper-V.

Para obter mais informações sobre o System Center 2016, consulte [System center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/).

> [!NOTE]
> Se você não quiser usar o System Center 2016, poderá usar o Windows PowerShell ou outro aplicativo de gerenciamento para instalar e configurar o controlador de rede e outra infraestrutura SLB. Para obter mais informações, consulte [implantar o controlador de rede usando o Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).

### <a name="network-controller"></a>Controlador de rede
O controlador de rede hospeda o Gerenciador SLB e executa as seguintes ações para SLB.

-   Processa comandos SLB que entram por meio da API Northbound do System Center, do Windows PowerShell ou de outro aplicativo de gerenciamento de rede.

-   Calcula a política de distribuição para hosts Hyper-V e SLB MUXes.

-   Fornece o status de integridade da infraestrutura SLB.

### <a name="slb-mux"></a>MUX SLB
O SLB MUX processa o tráfego de rede de entrada e mapeia VIPs para DIPs e encaminha o tráfego para o DIP correto. Cada MUX também usa o BGP para publicar rotas VIP para roteadores de borda. O BGP Keep Alive notifica o MUXes quando um MUX falha, o que permite que o Active MUXes redistribua a carga no caso de uma falha de MUX, basicamente fornecendo balanceamento de carga para os balanceadores de carga.

### <a name="hosts-that-are-running-hyper-v"></a>Hosts que executam o Hyper-V
Você pode usar o SLB com computadores que executam o Windows Server 2016 e o Hyper-V. As VMs no host Hyper-V podem executar qualquer sistema operacional com suporte no Hyper-V.

### <a name="slb-host-agent"></a>Agente de host SLB
Ao implantar o SLB, você deve usar o System Center, o Windows PowerShell ou outro aplicativo de gerenciamento para implantar o agente de host SLB em cada computador host Hyper-V. Você pode instalar o agente de host SLB em todas as versões do Windows Server 2016 que fornecem suporte ao Hyper-V, incluindo o nano Server.

O agente de host SLB escuta atualizações de política SLB do controlador de rede. Além disso, as regras de programas de agente de host para SLB nos comutadores virtuais Hyper-V habilitados para SDN configuradas no computador local.

### <a name="sdn-enabled-hyper-v-virtual-switch"></a>Comutador virtual Hyper-V habilitado para SDN
Para que um comutador virtual seja compatível com o SLB, você deve usar os comandos do Gerenciador de comutador virtual do Hyper-V ou do Windows PowerShell para criar o comutador e, em seguida, deve habilitar o VFP (plataforma de filtragem virtual) para o comutador virtual.

Para obter informações sobre como habilitar o VFP em comutadores virtuais, consulte os comandos do Windows PowerShell [Get-VMSystemSwitchExtension](/powershell/module/hyper-v/get-vmsystemswitchextension?view=win10-ps) e [Enable-VMSwitchExtension](/powershell/module/hyper-v/enable-vmswitchextension?f=255&MSPPError=-2147217396&view=win10-ps).

O comutador virtual Hyper-V habilitado para SDN executa as seguintes ações para SLB.

-   Processa o caminho de dados para SLB.

-   Recebe o tráfego de rede de entrada do MUX.

-   Ignora o MUX para o tráfego de rede de saída, enviando-o ao roteador usando DSR.

-   É executado em instâncias do nano Server do Hyper-V.

### <a name="bgp-enabled-router"></a>Roteador habilitado para BGP
O roteador BGP executa as seguintes ações para SLB.

-   Roteia o tráfego de entrada para o MUX usando ECMP.

-   Para o tráfego de rede de saída, o usa a rota fornecida pelo host.

-   Escuta atualizações de rota para VIPs de SLB MUX.

-   Remove o SLB MUXes da rotação SLB se Keep Alive falhar.

## <a name="software-load-balancing-features"></a><a name="bkmk_features"></a>Recursos de balanceamento de carga de software
A seguir estão alguns dos recursos e funcionalidades do SLB.

**Funcionalidade básica**

-   O SLB fornece serviços de balanceamento de carga de camada 4 para o tráfego de TCP/UDP "Norte-Sul" e "leste-oeste"

-   Você pode usar SLB em uma rede baseada em virtualização de rede Hyper-V

-   Você pode usar SLB com uma rede baseada em VLAN para VMs DIP conectadas a um comutador virtual Hyper-V habilitado para SDN.

-   Uma instância SLB pode lidar com vários locatários

-   O SLB e o DIP dão suporte a um caminho de retorno escalonável e de baixa latência, conforme implementado pelo retorno de servidor direto (DSR)

-   O SLB funciona quando você também usa o switch Embedded Integration (SET) ou a virtualização de entrada/saída de raiz única (SR-IOV)

-   SLB inclui suporte a IPv4 (protocolo IP versão 4)

-   Para cenários de gateway site a site, o SLB fornece a funcionalidade NAT para permitir que todas as conexões site a site utilizem um único IP público

-   Você pode instalar o SLB, incluindo o agente do host e o MUX, em instalação do Windows Server 2016, Full, Core e nano.

**Escala e desempenho**

-   Pronto para a escala de nuvem, incluindo capacidade de expansão e capacidade de expansão para MUXes e agentes de host.

-   Um módulo de controlador de rede ativo do Gerenciador SLB pode dar suporte a 8 instâncias do MUX

**Alta disponibilidade**

-   Você pode implantar o SLB em mais de 2 nós em uma configuração ativa/ativa

-   O MUXes pode ser adicionado e removido do pool de MUX sem afetar o serviço SLB. Isso mantém a disponibilidade de SLB quando MUXes individuais estão sendo corrigidos.

-   As instâncias individuais do MUX têm um tempo de atividade de 99%

-   Os dados de monitoramento de integridade estão disponíveis para entidades de gerenciamento

**Alinhamento**

-   Você pode implantar e configurar o SLB com o SCVMM

-   O SLB fornece uma borda unificada multilocatário, integrando-se diretamente a dispositivos da Microsoft, como o gateway de multilocatário do RAS, o Firewall do datacenter e o refletor de rota.