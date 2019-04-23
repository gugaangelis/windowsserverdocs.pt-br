---
title: Balanceamento de carga do software (SLB) para SDN
description: Você pode usar este tópico para saber mais sobre o balanceamento de carga de Software para a rede definida pelo Software no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 26fb4aa21e80618c4c63bd9edbf8731bf886db62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853757"
---
# <a name="software-load-balancing-slb-for-sdn"></a>O balanceamento de carga de software \(SLB\) para SDN

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre o balanceamento de carga de Software para a rede definida pelo Software no Windows Server 2016.  

Provedores de serviços de nuvem (CSPs) e empresas que estão implantando o Software Defined Networking (SDN) no Windows Server 2016 podem usar o balanceamento de carga de Software (SLB) para distribuir uniformemente o tráfego de rede de cliente do locatário entre os recursos de rede virtual e locatário. O SLB do Windows Server permite que vários servidores hospedem a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade.
  
Windows Server SLB inclui os seguintes recursos.  
  
-   Para tráfego TCP/UDP 'Leste-Oeste' e 'Norte-Sul' os serviços de balanceamento de carga de camada 4 (L4).  
  
-   Públicos e internos balanceamento de carga do tráfego de rede.  
  
-   Dá suporte a endereços IP dinâmicos (DIPs) em redes locais virtuais (VLANs) e em redes virtuais que você cria usando a virtualização de rede do Hyper-V.  
  
-   Suporte de investigação de integridade.  
  
-   Pronto para a escala de nuvem, incluindo a capacidade de expandir e escalar verticalmente a capacidade para multiplexadores e agentes de Host.  
  
Para obter mais informações, consulte [recursos de balanceamento de carga de Software](#bkmk_features) neste tópico.  
  
> [!NOTE]  
> Multilocação para VLANs não é suportado pelo controlador de rede, porém você pode usar VLANs com SLB para provedor de serviço gerenciado de cargas de trabalho, como a infraestrutura de datacenter e servidores Web de alta densidade.  
  
Usando o Windows Server SLB, você pode expandir seu recursos usando VMs SLB nos mesmos servidores de computação do Hyper-V que você usa para suas outras cargas de trabalho VM de balanceamento de carga. Por causa disso, o SLB dá suporte a rápida criação e exclusão de pontos de extremidade de balanceamento de carga que é necessária para operações de CSP. Além disso, o Windows Server SLB dá suporte a dezenas de gigabytes por cluster, fornece um modelo de provisionamento simples e é fácil de redução e escala horizontal.  
  
**Como funciona o SLB**  
  
SLB funciona pelo mapeamento de endereços IP virtuais (VIPs) para endereços IP dinâmicos (DIPs) são parte de um conjunto de serviço de nuvem de recursos no datacenter.  
  
VIPs são endereços IP simples que fornecem acesso público a um pool de carga balanceada VMs. Por exemplo, VIPs são endereços IP que são expostos na Internet para que os locatários e clientes do locatário podem se conectar aos recursos de locatário no datacenter de nuvem.  
  
Quedas são os endereços IP das VMs de um pool de balanceamento de carga por trás do VIP do membro. DIPs são atribuídos dentro da infraestrutura de nuvem para os recursos do locatário.  
  
VIPs são localizados no SLB multiplexador (MUX).  O MUX consiste em um ou mais máquinas virtuais (VMs).  Controlador de rede fornece a cada MUX com cada VIP e cada MUX por sua vez usam Border Gateway Protocol (BGP) para cada VIP para os roteadores na rede física como um/32 de anunciar rotas.  BGP permite que os roteadores de rede física:  
  
-   Saiba o que um VIP está disponível em cada MUX, mesmo se o MUXes estiverem em sub-redes diferentes em uma rede de camada 3.  
  
-   Distribuir a carga para cada VIP entre todos os MUXes disponíveis usando o roteamento de múltiplos caminhos de custo igual (ECMP).  
  
-   Automaticamente detectar uma falha MUX ou a remoção e parar de enviar tráfego para o MUX com falha.  
  
-   Distribuir a carga de MUX com falha ou removido entre o MUXes íntegro.  
  
Quando chega o tráfego público da Internet, o SLB MUX examina o tráfego, que contém o VIP como um destino e mapeia e reescreve o tráfego para que ele chegará em um DIP individual. Para tráfego de rede, essa transação é executada em um processo de duas etapas é dividido entre as MUX VMs (máquinas virtuais) e o host do Hyper-V onde se encontra o DIP de destino:  
  
-   Balanceamento de carga - os usos MUX o VIP para selecionar um DIP, encapsula o pacote e encaminha o tráfego para o host do Hyper-V onde se encontra o DIP.  
  
-   Rede (NAT) - conversão de endereços de host do Hyper-V remove o encapsulamento de pacote, converte o VIP para um DIP, remapeia as portas e encaminha o pacote para a VM de DIP.  
  
O MUX sabe como mapear os VIPs para os DIPs corretos devido a políticas que você define usando o controlador de rede de balanceamento de carga. Essas regras incluem o protocolo, porta de front-end, porta de Back-end e o algoritmo de distribuição (2, 3 ou 5 tuplas).  
  
Quando respondem de VMs de locatário e enviar a saída tráfego de rede para a Internet ou locais de locatários remotos, porque o NAT está sendo executado pelo host do Hyper-V, o tráfego ignora o MUX e vai diretamente para o roteador de borda do host do Hyper-V. Esse processo de bypass MUX é chamado de retorno de servidor direto (DSR).  
  
E, depois de estabelecer o fluxo de tráfego de rede inicial, o tráfego de rede de entrada ignora completamente o SLB MUX.  
  
Na ilustração a seguir, um computador cliente executa uma consulta DNS para o endereço IP de um empresa site do SharePoint – nesse caso, uma empresa fictícia chamada Contoso. O seguinte processo ocorre.  
  
-   O servidor DNS retorna o VIP 107.105.47.60 ao cliente.  
  
-   O cliente envia uma solicitação HTTP para o VIP.  
  
-   A rede física tem vários caminhos disponíveis para acessar o VIP localizado em qualquer MUX.  Cada roteador ao longo do caminho usa ECMP para escolher o próximo segmento do caminho até que a solicitação chega a um MUX.  
  
-   O MUX que recebe a solicitação verifica as políticas configuradas e vê que há duas queda disponível, 10.10.10.5 e 10.10.20.5, em uma rede virtual para lidar com a solicitação para o VIP 107.105.47.60  
  
-   O MUX seleciona o DIP 10.10.10.5 e encapsula os pacotes usando VXLAN, portanto, ele pode enviá-lo para o host que contém o DIP usando os hosts do endereço de rede física.  
  
-   O host recebe o pacote encapsulado e inspeciona a ele.  Remove o encapsulamento e reconfigure o pacote para que o destino é agora o DIP 10.10.10.5 em vez do VIP e envia o tráfego para a VM de DIP.  
  
-   Agora, a solicitação atingiu o site do SharePoint de Contoso em 2 de Farm de servidor. O servidor gera uma resposta e o envia para o cliente, usando seu próprio endereço IP como a origem.  
  
-   O host intercepta o pacote de saída no comutador virtual que se lembra de que o cliente, agora é o destino, que fez a solicitação original para o VIP.  O host regrava a origem do pacote seja o VIP, de modo que para o cliente não vê o endereço DIP.  
  
-   O host encaminha o pacote diretamente para o gateway padrão para a rede física que usa sua tabela de roteamento padrão para encaminhar o pacote de logon no cliente que, eventualmente, recebe a resposta.  
  
![Processo de balanceamento de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**Tráfego de datacenter interno de balanceamento de carga**  
  
Quando a carga do tráfego de rede balanceamento interno para o datacenter, tais como entre os recursos de locatário que estão em execução em servidores diferentes e os membros da mesma rede virtual, o comutador Virtual do Hyper-V para o qual as VMs estão conectadas executa NAT.  
  
Com balanceamento de carga do tráfego interno, a primeira solicitação é enviada ao e processada pelo MUX, que seleciona o DIP apropriado e encaminha o tráfego para o DIP. A partir desse ponto, o fluxo de tráfego estabelecido ignora o MUX e vai diretamente da VM para VM.  
  
**Investigações de integridade**  
  
SLB inclui as investigações de integridade para validar a integridade da infraestrutura de rede, incluindo o seguinte.  
  
-   Porta de investigação de TCP  
  
-   Investigação HTTP para a porta e URL  
  
Ao contrário de um dispositivo de Balanceador de carga tradicional em que a investigação se origina no dispositivo e viagem pelos fios para o DIP, a investigação SLB se origina no host em que o DIP é localizado e vai diretamente do agente de host SLB para o DIP ainda mais distribuindo o trabalhar em hosts.  
  
## <a name="bkmk_infrastructure"></a>Infraestrutura de balanceamento de carga de software  
Para implantar o Windows Server SLB, é necessário implantar primeiro o controlador de rede no Windows Server 2016 e uma ou mais VMs MUX de SLB.  
  
Além disso, você deve configurar hosts Hyper-V com o comutador Virtual Hyper-V SDN habilitado e certifique-se de que o agente de Host SLB está em execução.  Os roteadores que servem os hosts devem dar suporte a roteamento de vários caminhos (ECMP) custo igual e Border Gateway Protocol (BGP) e devem ser configurados para aceitar solicitações de emparelhamento via protocolo BGP do MUXes SLB.  
  
A seguir está uma visão geral da infraestrutura do SLB.  

![Infraestrutura de balanceamento de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
As seções a seguir fornecem mais informações sobre esses elementos da infraestrutura do SLB.  
  
### <a name="scvmm"></a>SCVMM  
Com o System Center 2016, você pode configurar o controlador de rede no Windows Server 2016, incluindo o Gerenciador de SLB e um Monitor de integridade. Você também pode usar o System Center para implantar o SLB MUXs e instalar agentes de Host do SLB em computadores que executam o Windows Server 2016 e do Hyper-V.  
  
Para obter mais informações sobre o System Center 2016, consulte [System Center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/).  
  
> [!NOTE]  
> Se você não quiser usar o System Center 2016, você pode usar o Windows PowerShell ou outro aplicativo de gerenciamento para instalar e configurar o controlador de rede e outras infraestruturas SLB. Para obter mais informações, consulte [implantar o controlador de rede usando o Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="network-controller"></a>Controlador de rede  
Controlador de rede hospeda o Gerenciador de SLB e executa as seguintes ações para SLB.  
  
-   Comandos SLB de processos que são fornecidos por meio da API Northbound do System Center, Windows PowerShell ou outro aplicativo de gerenciamento de rede.  
  
-   Calcula a política para distribuição aos hosts do Hyper-V e MUXes SLB.  
  
-   Fornece o status de integridade da infraestrutura do SLB.  
  
### <a name="slb-mux"></a>SLB MUX  
O SLB MUX processa o tráfego de rede mapeia VIPs para DIPs e encaminha o tráfego para o DIP correto. Cada MUX também usa o BGP para publicar as rotas de VIP com roteadores de borda. BGP "Keep Alive" notifica MUXes quando um MUX falha, o que permite que o Active Directory MUXes redistribuir a carga em caso de falha MUX - essencialmente fornecendo balanceamento de carga para os balanceadores de carga.  
  
### <a name="hosts-that-are-running-hyper-v"></a>Hosts que executam o Hyper-V  
Você pode usar o SLB com computadores que executam o Windows Server 2016 e do Hyper-V. As VMs no host Hyper-V podem executar qualquer sistema operacional com suporte de Hyper-V.  
  
### <a name="slb-host-agent"></a>Agente de Host SLB  
Quando você implanta o SLB, você deve usar o System Center, Windows PowerShell ou outro aplicativo de gerenciamento para implantar o SLB Host Agent em cada computador de host do Hyper-V. Você pode instalar o agente de Host SLB em todas as versões do Windows Server 2016 que dão suporte a Hyper-V, incluindo o Nano Server.  
  
O agente de Host do SLB escuta para atualizações de política SLB do controlador de rede. Além disso, o agente de host programas regras para SLB no SDN habilitado do Hyper-V comutadores virtuais que são configurados no computador local.  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN habilitado o comutador Virtual Hyper-V  
Para um comutador virtual ser compatível com o SLB, você deve usar comandos do Gerenciador de comutador Virtual Hyper-V ou o Windows PowerShell para criar o comutador e, em seguida, você deve habilitar Virtual filtragem VFP (plataforma) para o comutador virtual.  
  
Para obter informações sobre como habilitar VFP em comutadores virtuais, consulte os comandos do Windows PowerShell [Get-VMSystemSwitchExtension](https://technet.microsoft.com/library/hh848603.aspx) e [Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx?f=255&MSPPError=-2147217396).  
  
A SDN habilitado o comutador Virtual Hyper-V executa as seguintes ações para SLB.  
  
-   Processa o caminho de dados para SLB.  
  
-   Recebe o tráfego de rede de MUX.  
  
-   Ignora o MUX para tráfego de rede de saída, enviá-la para o roteador usando DSR.  
  
-   É executado em instâncias do Nano Server do Hyper-V.  
  
### <a name="bgp-enabled-router"></a>Roteador do BGP habilitado  
O roteador BGP executa as seguintes ações para SLB.  
  
-   Roteia o tráfego de entrada para o MUX usando ECMP.  
  
-   Para tráfego de rede de saída, usa a rota fornecida pelo host.  
  
-   Escuta atualizações de rota para VIPs de SLB MUX.  
  
-   Remove MUXes SLB da rotação de SLB se falhar "Keep Alive".  
  
## <a name="bkmk_features"></a>Recursos de balanceamento de carga de software  
A seguir estão alguns dos recursos e funcionalidades do SLB.  
  
**Funcionalidade principal**  
  
-   SLB fornece serviços para tráfego TCP/UDP 'Leste-Oeste' e 'Norte-Sul' de balanceamento de carga de camada 4  
  
-   Você pode usar o SLB em uma rede baseada em virtualização de rede do Hyper-V  
  
-   Você pode usar SLB com uma rede baseada em VLAN para VMs de DIP conectado a um SDN habilitado do Hyper-V Switch Virtual.  
  
-   Uma instância SLB pode lidar com vários locatários  
  
-   SLB e DIP dar suporte a um caminho de retorno dimensionável e de baixa latência, conforme implementado pelo servidor de DSR (retorno direto)  
  
-   Funções SLB quando você também estiver usando o Switch Embedded Teaming (SET) ou virtualização de entrada/saída de raiz única (SR-IOV)  
  
-   Protocolo IP versão 4 (IPv4) inclui o SLB suporte  
  
-   Para cenários de gateway site a site, o SLB fornece funcionalidade NAT para habilitar todas as conexões site a site utilizar um único IP público  
  
-   Você pode instalar o SLB, incluindo o agente de Host e o MUX, no Windows Server 2016, Full, Core e instalação do Nano.  
  
**Escala e desempenho**  
  
-   Pronto para a escala de nuvem, incluindo a capacidade de expandir e escalar verticalmente a capacidade para MUXes e agentes de Host.  
  
-   Um módulo Active Directory do controlador de rede do Gerenciador de SLB pode dar suporte a 8 instâncias MUX  
  
**Alta disponibilidade**  
  
-   Você pode implantar o SLB para mais de 2 nós em uma configuração ativo/ativo  
  
-   MUXes podem ser adicionados e removidos do pool de MUX sem afetar o serviço SLB. Isso mantém a disponibilidade SLB ao   
    MUXes individuais estão sendo corrigidos.  
  
-   Instâncias MUX individuais têm um tempo de atividade de 99%  
  
-   Os dados de monitoramento de integridade está disponível para entidades de gerenciamento  
  
**Alinhamento**  
  
-   Você pode implantar e configurar o SLB com o SCVMM  
  
-   SLB fornece uma borda unificada multilocatária integrando-se perfeitamente com soluções da Microsoft, como o Gateway do multilocatário do RAS, o Firewall do Datacenter e o refletor de rota.  
  

