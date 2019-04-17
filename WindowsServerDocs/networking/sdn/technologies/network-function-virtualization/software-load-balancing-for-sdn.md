---
title: Software balanceamento de carga (SLB) para SDN
description: Você pode usar este tópico para saber mais sobre balanceamento de carga de Software para Software definido de rede no Windows Server 2016.
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
ms.openlocfilehash: 5e08afddde9c7be8d955a0cfdaf44f0fc31b8155
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="software-load-balancing-slb-for-sdn"></a>Software balanceamento de carga \(SLB\) para SDN

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre balanceamento de carga de Software para Software definido de rede no Windows Server 2016.  

Provedores de serviço de nuvem (CSPs) e as empresas que estão implantando Software definida de rede (SDN) no Windows Server 2016 podem usar o balanceamento de carga de Software (SLB) para distribuir uniformemente locatário e tráfego de rede de cliente de locatário entre recursos de rede virtual. O Windows Server SLB permite que vários servidores hospedar a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade.
  
Windows Server SLB inclui os seguintes recursos.  
  
-   Camada 4 (L4) carregar serviços balanceamento para 'Norte-Sul' e 'Leste-Oeste' TCP/UDP tráfego.  
  
-   Public e Internal balanceamento de carga do tráfego de rede.  
  
-   Dá suporte a endereços IP dinâmicos (DIPs) em redes locais virtuais (VLANs) e em redes virtuais que você cria usando a virtualização de rede do Hyper-V.  
  
-   Suporte de teste de integridade.  
  
-   Pronto para escala de nuvem, incluindo a capacidade de dimensionamento e aumentar o recurso multiplexers e agentes de Host.  
  
Para obter mais informações, consulte [recursos de balanceamento de carga de Software](#bkmk_features) neste tópico.  
  
> [!NOTE]  
> Multitenancy para VLANs não é suportado pelo controlador de rede, mas você pode usar VLANs com SLB para o provedor de serviço gerenciado cargas de trabalho, como a infraestrutura de datacenter e servidores Web de alta densidade.  
  
Usando o Windows Server SLB, você pode dimensionar seu recursos usando SLB VMs nos mesmos servidores de computação do Hyper-V que você usa para suas outras cargas de trabalho VM de balanceamento de carga. Por isso, SLB dá suporte a criação rápida e a exclusão de balanceamento de carga de pontos de extremidade que é necessária para operações de CSP. Além disso, Windows Server SLB dá suporte a dezenas de gigabytes por cluster, fornece um modelo simples de provisionamento e é fácil de escala e em.  
  
**Como funciona o SLB**  
  
SLB funciona por meio do mapeamento endereços IP virtuais (VIPs) a endereços IP dinâmicos (DIPs) que fazem parte de um conjunto de serviço de nuvem de recursos no datacenter.  
  
VIPs são endereços IP simples que fornecem acesso público a um pool de carga balanceada VMs. Por exemplo, VIPs são endereços IP que são expostos na Internet para que locatários e clientes de locatário podem se conectar aos recursos de locatário no datacenter na nuvem.  
  
DIPs são os endereços IP do membro VMs de um pool de carga balanceada atrás VIP. DIPs são atribuídas dentro da infraestrutura de nuvem para os recursos de locatário.  
  
VIPs estão localizadas em SLB multiplexador (Multiplexador).  Multiplexador consiste em um ou mais VMs (máquinas virtuais).  Controlador de rede fornece cada MUX com cada VIP e cada Multiplexador sucessivamente usam Border Gateway Protocol (BGP) para anunciar cada VIP para roteadores na rede física, como um/32 rota.  BGP permite que os roteadores de rede física para:  
  
-   Saiba que um VIP está disponível em cada Multiplexador, mesmo que o MUXes estejam em várias sub-redes em uma rede de camada 3.  
  
-   Distribuir a carga para cada VIP em todos os MUXes disponíveis usando o roteamento de vários caminhos de custo igual (ECMP).  
  
-   Detectar uma falha de Multiplexador ou remoção automaticamente e parar de enviar tráfego para MUX com falha.  
  
-   Distribuir a carga de Multiplexador com falha ou são removido entre o MUXes íntegro.  
  
Quando chega tráfego público da Internet, SLB MUX examina o tráfego, que contém o VIP como um destino e mapas e reconfigure o tráfego para que ele chegarão a um DIP individual. Para o tráfego de rede de entrada, esta transação é realizada em um processo de duas etapas é dividido entre as Multiplexador VMs (máquinas virtuais) e o host do Hyper-V onde se encontra o destino DIP:  
  
-   Balanceamento de carga - os usos de Multiplexador VIP para selecionar um DIP, encapsula o pacote e encaminha o tráfego para o host do Hyper-V onde se encontra a DIP.  
  
-   Rede NAT (Address Translation) - o host do Hyper-V remove o encapsulamento do pacote, traduz VIP a um DIP, remapeia as portas e encaminha o pacote para a VM DIP.  
  
Multiplexador sabe como mapear VIPs para os corretos DIPs devido a políticas que você define usando o controlador de rede de balanceamento de carga. Essas regras incluem protocolo, porta front-end, porta de Back-end e o algoritmo de distribuição (2, 3 ou 5 tuplas).  
  
Quando locatário VMs respondem e enviar saída tráfego de rede de volta para a Internet ou locais remotos de locatário, porque a NAT é realizado pelo host do Hyper-V, o tráfego ignora Multiplexador e vai diretamente para o roteador de borda do host do Hyper-V. Esse processo de ignorar Multiplexador é chamado retornar de servidor direto (DSR).  
  
E, depois que o fluxo de tráfego de rede inicial é estabelecido, o tráfego de rede de entrada ignora SLB MUX completamente.  
  
Na ilustração a seguir, um computador cliente executa uma consulta DNS para o endereço IP de um site Sharepoint da empresa - neste caso, uma empresa fictícia chamada Contoso. O seguinte processo ocorre.  
  
-   O servidor DNS retorna o VIP 107.105.47.60 ao cliente.  
  
-   O cliente envia uma solicitação HTTP para o VIP.  
  
-   A rede física tem vários caminhos disponíveis para alcançar o VIP localizado em qualquer Multiplexador.  Cada roteador ao longo do caminho usa ECMP para selecionar o próximo segmento do caminho até que a solicitação chega ao quarto de um Multiplexador.  
  
-   Multiplexador que recebe a solicitação verifica políticas configuradas e vê que existem dois DIPs disponíveis, 10.10.10.5 e 10.10.20.5, em uma rede virtual para lidar com a solicitação para o VIP 107.105.47.60  
  
-   Multiplexador seleciona a DIP 10.10.10.5 e encapsula os pacotes usando VXLAN para que ele possa enviá-lo para o host que contém a DIP usando os hosts endereços de rede física.  
  
-   O host recebe o pacote encapsulado e inspeciona.  Ele remove o encapsulamento e reconfigure o pacote para que o destino é agora o DIP 10.10.10.5 em vez de VIP e envia o tráfego para DIP VM.  
  
-   A solicitação agora alcançou o site do Contoso Sharepoint Server Farm 2. O servidor gera uma resposta e o envia para o cliente, usando seu próprio endereço IP como a origem.  
  
-   O host intercepta o pacote de saída no switch virtual que reconhece que o cliente, agora de destino, que fez a solicitação original para o VIP.  O host reconfigura a origem do pacote para ser o VIP para que o cliente não ver o endereço DIP.  
  
-   O host encaminha o pacote diretamente para o gateway padrão para a rede física que usa sua tabela de roteamento padrão para encaminhar o pacote para o cliente que eventualmente recebe a resposta.  
  
![Processo de balanceamento de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**Carregar balanceamento tráfego datacenter interno**  
  
Quando carregar balanceamento o tráfego de rede interno para o datacenter, como entre recursos de locatário que executam em servidores diferentes e que são membros da mesma rede virtual, o comutador Virtual Hyper-V para o qual as VMs estão conectadas executa NAT.  
  
Com balanceamento de carga do tráfego interno, a primeira solicitação é enviada para e processada pelo Multiplexador, que seleciona a DIP apropriado e encaminha o tráfego para a DIP. Desse ponto, o fluxo de tráfego estabelecida ignora Multiplexador e vai diretamente da VM a VM.  
  
**Testes de integridade**  
  
SLB inclui testes de integridade para validar a integridade da infraestrutura de rede, incluindo o seguinte.  
  
-   Teste TCP à porta  
  
-   Teste HTTP para porta e URL  
  
Ao contrário de um dispositivo de Balanceador de carga tradicional em que o teste se origina no dispositivo e passa pela conexão para a DIP, o teste SLB se origina no host onde a DIP está localizado e vai diretamente do agente do host SLB para DIP, distribuir ainda mais o trabalho em todos os hosts.  
  
## <a name="bkmk_infrastructure"></a>Infraestrutura de balanceamento de carga de software  
Para implantar o Windows Server SLB, primeiro você deve implantar controlador de rede no Windows Server 2016 e um ou mais VMs do Multiplexador SLB.  
  
Além disso, você deve configurar hosts Hyper-V com o comutador Virtual habilitado SDN Hyper-V e certifique-se de que o agente de Host SLB está sendo executado.  Os roteadores que servem os hosts devem ter suporte para roteamento de vários caminhos (ECMP) igual custo e Border Gateway Protocol (BGP) e devem ser configurados para aceitar solicitações de correspondência BGP de MUXes o SLB.  
  
A seguir é uma visão geral da infraestrutura SLB.  

![Infraestrutura de balanceamento de carga de software](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
As seções a seguir fornecem mais informações sobre esses elementos da infraestrutura SLB.  
  
### <a name="scvmm"></a>SCVMM  
Com o System Center 2016, você pode configurar o controlador de rede no Windows Server 2016, incluindo o Gerenciador de SLB e o Monitor de saúde. Você também pode usar o System Center para implantar SLB MUXs e instalar agentes de Host SLB em computadores que executam o Windows Server 2016 e Hyper-V.  
  
Para obter mais informações sobre o System Center 2016, consulte [System Center 2016](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/).  
  
> [!NOTE]  
> Se você não quiser usar o System Center 2016, você pode usar o Windows PowerShell ou outro aplicativo de gerenciamento para instalar e configurar o controlador de rede e outra infraestrutura SLB. Para obter mais informações, consulte [implantar o controlador de rede usando o Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="network-controller"></a>Controlador de rede  
Controlador de rede hospeda o Gerenciador de SLB e executa as seguintes ações para SLB.  
  
-   Comandos SLB de processos que vêm em por meio da API em sentido norte do System Center, Windows PowerShell ou outro aplicativo de gerenciamento de rede.  
  
-   Calcula a política para distribuição a hosts Hyper-V e SLB MUXes.  
  
-   Fornece o status de integridade da infraestrutura SLB.  
  
### <a name="slb-mux"></a>MULTIPLEXADOR SLB  
O SLB MUX processa o tráfego da rede e VIPs é mapeado para DIPs e encaminha o tráfego para a DIP correto. Cada Multiplexador também usa BGP para publicar rotas VIP roteadores de borda. BGP Keep Alive notifica MUXes quando um Multiplexador falha, o que permite que o active MUXes redistribua a carga em caso de uma falha de Multiplexador - essencialmente fornecendo balanceamento de carga para os balanceadores de carga.  
  
### <a name="hosts-that-are-running-hyper-v"></a>Hosts que executam o Hyper-V  
Você pode usar SLB com computadores que executam o Windows Server 2016 e Hyper-V. VMs no host do Hyper-V podem executar qualquer sistema operacional compatível com o Hyper-V.  
  
### <a name="slb-host-agent"></a>Agente de Host SLB  
Quando você implanta SLB, você deve usar o System Center, Windows PowerShell ou outro aplicativo de gerenciamento para distribuir o agente de Host SLB em cada computador de host do Hyper-V. Você pode instalar o agente de Host SLB em todas as versões do Windows Server 2016 que oferecem suporte a Hyper-V, incluindo Nano Server.  
  
O agente de Host SLB escuta de atualizações de política SLB de controlador de rede. Além disso, o agente de host programas regras para SLB no SDN Switches compatíveis com o Hyper-V Virtual que estão configurados no computador local.  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN habilitado comutador Virtual Hyper-V  
Para um comutador virtual ser compatível com SLB, você deve usar comandos de Gerenciador do Hyper-V Virtual alternar ou o Windows PowerShell para criar o switch e, em seguida, você deve habilitar a plataforma de filtragem Virtual (VFP) para o comutador virtual.  
  
Para obter informações sobre como habilitar VFP em comutadores virtuais, ver os comandos do Windows PowerShell [Get-VMSystemSwitchExtension](https://technet.microsoft.com/en-us/library/hh848603.aspx) e [habilitar VMSwitchExtension](https://technet.microsoft.com/en-us/library/hh848541.aspx?f=255&MSPPError=-2147217396).  
  
O SDN habilitado Hyper-V Virtual Switch executa as seguintes ações para SLB.  
  
-   Processa o caminho dos dados para SLB.  
  
-   Recebe o tráfego de rede de entrada do Multiplexador.  
  
-   Ignora Multiplexador para o tráfego de rede de saída, enviá-lo ao roteador usando DSR.  
  
-   É executado em instâncias de servidor Nano do Hyper-V.  
  
### <a name="bgp-enabled-router"></a>BGP habilitado roteador  
O roteador BGP executa as seguintes ações para SLB.  
  
-   Rotas tráfego de entrada Multiplexador usando ECMP.  
  
-   Para o tráfego de rede de saída, usa a rota fornecida pelo host.  
  
-   Ouve para atualizações de rota VIPs de SLB MUX.  
  
-   Remove SLB MUXes a rotação SLB se falhar Keep Alive.  
  
## <a name="bkmk_features"></a>Recursos de balanceamento de carga de software  
A seguir estão alguns dos recursos e funcionalidades de SLB.  
  
**Funcionalidade básica**  
  
-   SLB fornece serviços para 'Norte-Sul' e 'Leste-Oeste' TCP/UDP tráfego de balanceamento de carga de camada 4  
  
-   Você pode usar SLB em uma rede baseada em virtualização de rede do Hyper-V  
  
-   Você pode usar SLB com uma rede baseada em VLAN para VMs DIP conectado a um SDN habilitado Hyper-V comutador Virtual.  
  
-   Uma instância SLB pode manipular vários locatários  
  
-   SLB e DIP dar suporte a um caminho de retorno escalável e de baixa latência, conforme implementada pelo direta servidor retorne (DSR)  
  
-   Funções SLB quando você estiver usando também agrupamento Embedded Switch (conjunto) ou virtualização de entrada/saída única raiz (SR-IOV)  
  
-   SLB inclui protocolo IP versão 4 (IPv4) suporte  
  
-   Para cenários de gateway to-site, SLB fornece funcionalidade NAT para permitir que todas as conexões to-site utilizar um único IP público  
  
-   Você pode instalar SLB, incluindo o agente do Host e Multiplexador, no Windows Server 2016, completo, Core e Nano instalar.  
  
**Dimensionamento e desempenho**  
  
-   Pronto para escala de nuvem, incluindo a capacidade de dimensionamento e aumentar o recurso MUXes e agentes de Host.  
  
-   Um módulo de controlador de rede SLB Manager ativo pode dar suporte a 8 instâncias de Multiplexador  
  
**Alta disponibilidade**  
  
-   Você pode implantar SLB em mais de 2 nós em uma configuração ativa/ativa  
  
-   MUXes podem ser adicionados e removidos do pool Multiplexador sem afetar o serviço SLB. Isso mantém a disponibilidade SLB quando   
    estão sendo corrigidos MUXes individuais.  
  
-   Instâncias individuais do Multiplexador tem um tempo de atividade de 99%  
  
-   Dados de monitoramento de integridade está disponível para entidades de gerenciamento  
  
**Alinhamento**  
  
-   Você pode implantar e configurar SLB com SCVMM  
  
-   SLB fornece uma borda unificada vários locatários integrando-se perfeitamente com dispositivos Microsoft como o RAS Multitenant Gateway, Datacenter Firewall e refletor de rota.  
  

