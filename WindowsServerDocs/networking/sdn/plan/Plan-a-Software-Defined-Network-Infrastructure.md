---
title: Planeje uma infraestrutura de rede definidos de Software
description: Este tópico fornece informações sobre como planejar a implantação de infraestrutura de rede de definido de Software (SDN).
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ea7e53c8-11ec-410b-b287-897c7aaafb13
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bb3b9313996637fa5ee7367c538fe04d7cbefea9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Planeje uma infraestrutura de rede definidos de Software

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Examine as seguintes informações para ajudar a planejar sua implantação de infraestrutura de rede de definido de Software (SDN). Depois que você examine essas informações, consulte [implantar uma infraestrutura de rede definidos do Software](../deploy/Deploy-a-Software-Defined-Network-Infrastructure.md) para obter informações de implantação.

>[!NOTE]
>Além de neste tópico, o seguinte conteúdo de planejamento SDN está disponível.  
>
> - [Instalação e requisitos de preparação para a implantação de controlador de rede](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  

Para obter informações sobre o Hyper-V rede virtualização (HNV), que você pode usar para virtualizar redes em uma implantação do Microsoft SDN, consulte [virtualização de rede do Hyper-V](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  

## <a name="prerequisites"></a>Pré-requisitos
Este tópico descreve um número de pré-requisitos de hardware e software, incluindo:

-   **Rede física**  
    Você precisa de acesso aos seus dispositivos de rede física para configurar VLANs, roteamento, BGP, dados central ponte (ETS) se usando uma tecnologia RDMA e tecnologia RDMA com base em dados central ponte (PFC) se usando um RoCE. Este tópico mostra a configuração manual switch, bem como BGP estranhas em switches de camada 3 / roteadores ou uma máquina virtual de roteamento e o servidor de acesso remoto (RRAS).   

-   **Hosts de computação físico**  
Esses hosts execute Hyper-V e são necessários para hospedar SDN infraestrutura e locatário máquinas virtuais.  Hardware de rede específica é necessário nesses hosts para obter melhor desempenho, que é descrita mais adiante a **hardware de rede** seção.  
      
  
## <a name="physical-network-configuration"></a>Configuração de rede física

Cada host físico computação requer conectividade de rede por meio de um ou mais adaptadores de rede conectado às portas comutador físico. A rede é separada em vários segmentos de rede lógica opcionalmente respaldos de camada 2 [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN). Os prefixos de sub-rede IP e IDs de VLAN mostrado a seguir são exemplos e devem ser personalizado para seu ambiente com base na orientação de seu administrador de rede. Se qualquer uma das suas redes lógicas são marcados ou no modo de acesso, use VLAN ID 0 para essas redes ao configurar as sub-redes lógicas em arquivos de configuração System Center Virtual Machine Manager ou o PowerShell script.

>[!IMPORTANT]
>Windows Server 2016 Software definido de rede oferece suporte a endereçamento IPv4 para a base e a sobreposição. Não há suporte para o IPv6.
  
### <a name="management-and-hnv-provider-logical-networks"></a>Gerenciamento e HNV provedor redes lógicas

Física todos os hosts precisam ter acesso à rede lógica gerenciamento e a rede lógica HNV provedor de computação. Se a redes lógicas usam VLANs, os hosts de computação físico devem estar conectado a uma porta de tronco switch que tem acesso a essas VLANs. Da mesma forma, os adaptadores de rede física no host computação não devem ter qualquer VLAN filtragem ativado. Se você estiver usando o agrupamento de Switch-Embedded (conjunto) e tiver vários membros da equipe NIC (ou seja, adaptadores de rede) em seu hosts de computação, você deve conectar todos os membros da equipe de placa de rede para que o host específico no mesmo domínio de transmissão de camada 2.  
  
Para fins de planejamento de endereço de IP, cada host físico de computação deve ter pelo menos um endereço IP atribuído da rede lógica gerenciamento. O controlador de rede atribui automaticamente exatamente dois endereços IP da rede lógica HNV provedor. Se o host de computação físico estiver executando máquinas virtuais de infraestrutura adicional (por exemplo, o controlador de rede, SLB/Multiplexador ou Gateway) esse host deve ter um endereço IP adicional atribuído a partir da rede de lógica de gerenciamento para cada uma das máquinas virtuais infraestrutura hospedadas.   
  
Além disso, cada VM de infraestrutura SLB/Multiplexador deve ter um endereço IP reservado da rede lógica HNV provedor. 

>[!IMPORTANT]
>Estes endereços IP SLB/Multiplexador devem ser atribuídos de fora do pool de endereços IP que esteja configurado para a rede lógica HNV provedor. Falha de fazer isso pode resultar em endereços IP duplicados na rede. 

O controlador de rede requer um endereço reservado da rede gerenciamento para servir como o endereço IP de REST. Você deve criar manualmente o registro de HOST A no DNS para o endereço IP de REST.  
  
Um servidor DHCP pode atribuir endereços IP para o gerenciamento de rede automaticamente ou você pode atribuir manualmente o endereço IP estático. A pilha SDN atribui automaticamente endereços IP para a rede de provedor HNV para os hosts Hyper-V individuais de um Pool de IP especificado por meio de e gerenciado pelo controlador de rede.   
  
O administrador de malha estaticamente atribui os endereços IP do provedor de HNV usados por SLB/Multiplexador por meio de scripts do PowerShell ou VMM. O controlador de rede atribui um endereço IP do provedor de HNV a um host de computação física somente depois que o agente de Host do controlador de rede recebe a política de rede para uma máquina virtual de locatário específico.  
  
#### <a name="sample-network-topology"></a>Topologia de rede de amostra

Personalize os prefixos de sub-rede, VLAN IDs e endereços IP de gateway com base na orientação do administrador da rede.  
  
Nome da rede|Sub-rede|Máscara|ID de VLAN em tronco|Gateway|Reservas<br />(exemplos)  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**Gerenciamento**|10.184.108.0|24|7|10.184.108.1|10.184.108.1 - roteador<br /><br />10.184.108.4 - controlador de rede<br /><br />10.184.108.10 - host de computação 1<br /><br />10.184.108.11 - host de computação 2<br /><br />10.184.108.X - host de computação X  
|**Provedor HNV**|10.10.56.0|23|11|10.10.56.1|10.10.56.1 - roteador<br /><br />10.10.56.2 - SLB/MUX1  
  
### <a name="logical-networks-for-gateways-and-the-software-load-balancer"></a>Redes lógicas para Gateways e o Software balanceador
  
Redes lógicas adicionais precisam ser criados e provisionada para uso SLB e gateway. Mais uma vez, você precisa trabalhar com o administrador de rede para obter os prefixos IP corretos, VLAN IDs e endereços IP de gateway para essas redes.

#### <a name="transit-logical-network"></a>Rede lógica de trânsito
  
O Gateway RAS e SLB/Multiplexador usam a rede lógica de trânsito para trocar informações de correspondência BGP e tráfego do Norte/South locatário (externo-interno). O tamanho dessa sub-rede normalmente será menor do que os outros. Somente os hosts de computação físico que executam RAS Gateway ou máquinas virtuais SLB/Multiplexador precisam ter conectividade com essa sub-rede com estas VLANs tronco e acessíveis nas portas alternar para o qual os adaptadores de rede dos hosts de computação estão conectados. Cada SLB/Multiplexador ou uma máquina virtual de Gateway RAS estaticamente é atribuída um endereço IP da rede lógica trânsito.

#### <a name="public-vip-logical-network"></a>Rede pública de lógica VIP  
  
Rede pública VIP lógica é necessária para ter os prefixos de sub-rede IP que estão pode ser roteados fora do ambiente de nuvem (geralmente pode ser roteado da Internet).  Eles serão os endereços IP front-end usados pelos clientes externos para acessar recursos em redes virtuais incluindo VIP front-end do gateway to-site.

#### <a name="private-vip-logical-network"></a>Rede privada de lógica VIP
  
Rede privada VIP lógica não é necessária para ser pode ser roteado fora da nuvem como ela é usada para VIPs que só são acessados de clientes de nuvem interno, como o SLB gerenciado ou serviços particulares.
  
#### <a name="gre-vip-logical-network"></a>Rede lógica GRE VIP

A rede GRE VIP é uma sub-rede que existe exclusivamente para definir VIPs que são designados para máquinas virtuais do gateway em execução no sua malha SDN para um tipo de conexão S2S GRE. Essa rede não precisam ser pré-configurado no seu roteador ou switches físicos e precisa não têm uma VLAN atribuída.   

### <a name="sample-network-topology"></a>Topologia de rede de amostra

Personalize os prefixos de sub-rede, VLAN IDs e endereços IP de gateway com base na orientação do administrador da rede.  
  
Nome da rede|Sub-rede|Máscara|ID de VLAN em tronco|Gateway|Reservas<br />(exemplos)  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**Transporte**|10.10.10.0|24|10|10.10.10.1|10.10.10.1 - roteador  
|**VIP pública**|41.40.40.0|27|NA|41.40.40.1|41.40.40.1 - roteador<br /> 41.40.40.2 - SLB/Multiplexador VIP<br />41.40.40.3 - IPSec S2S VPN VIP  
|**VIP privada**|20.20.20.0|27|NA|20.20.20.1|20.20.20.1 - GW padrão (roteador)  
|**GRE VIP**|31.30.30.0|24|NA|31.30.30.1|31.30.30.1 - GW padrão|  
  
### <a name="logical-networks-required-for-rdma-based-storage"></a>Redes lógicas necessários para armazenamento baseado em RDMA  
  
Se você estiver usar RDMA com base em armazenamento e você precisará definir um VLAN e sub-rede para cada adaptador físico em hosts de computação e armazenamento. Normalmente, você terá dois adaptadores físicos por nó para essa configuração.  
  
> [!IMPORTANT]  
> Switches mais físicos exigem tráfego RDMA seja enviado em uma VLAN marcada na ordem de qualidade de configurações de serviço sejam aplicadas corretamente.  Não coloque o tráfego RDMA em uma VLAN não rotulada ou em uma porta de modo de acesso físico.  
  
Nome da rede  |Sub-rede  |Máscara  |ID de VLAN em tronco  |Gateway  |Reservas<br />(exemplos)    
---------|---------|---------|---------|---------|---------  
**Storage1**     |    10.60.36.0     | 25        |   8      |  10.60.36.1       |  10.60.36.1 - roteador<br />10.60.36.x - host de computação x<br />10.60.36.y - y do host de computação<br />10.60.36.v - compute cluster<br />10.60.36.w - cluster de armazenamento  
|**Armazenamento2 do**|10.60.36.128|25|9|10.60.36.129|10.60.36.129 - roteador<br />10.60.36.x - host de computação x<br />10.60.36.y - y do host de computação<br />10.60.36.v - compute cluster<br />10.60.36.w - cluster de armazenamento  
  
Para obter mais informações sobre como configurar opções, consulte o **exemplos de configuração** seção.  
  
## <a name="routing-infrastructure"></a>Infraestrutura de roteamento  
  
Se você estiver implantando sua infraestrutura SDN usando scripts, as sub-redes gerenciamento, HNV provedor, trânsito e VIP devem ser pode ser roteadas uns aos outros na rede física.     
  
Informações de roteamento \ (por exemplo, Avançar-hop\) para o VIP sub-redes está anunciado pelo SLB/Multiplexador e RAS Gateways para a rede física usando a correspondência de BGP interno. As redes lógicas VIP não têm uma VLAN atribuída e não é pré-configurado no comutador de camada 2 (por exemplo, alternar superior de Rack).  
  
Você precisa criar um par BGP no roteador que é usado por sua infraestrutura SDN para receber rotas para as redes de lógica VIP anunciadas pelo SLB/MUXes e Gateways RAS. BGP estranhas só precisem ocorrer uma maneira (de SLB/Multiplexador ou RAS Gateway para o par BGP externo).  Acima a primeira camada de roteamento que você pode usar rotas estáticas ou outro protocolo de roteamento dinâmico, como OSPF, no entanto, afirmado anteriormente, o prefixo de sub-rede IP para as redes lógicas VIP precisam ser pode ser roteado da rede física para o par BGP externo.   
  
BGP estranhas normalmente é configurada em um switch gerenciado ou um roteador como parte da infraestrutura de rede. O par BGP também pode ser configurado em um servidor Windows com a função de servidor (acesso remoto) instalada em um modo somente de roteamento. Esse par de roteador BGP da infraestrutura de rede deve ser configurada para ter seu próprio ASN e permitir a correspondência de um ASN é atribuído aos componentes SDN \ (SLB/Multiplexador e RAS Gateways\). Você deve obter as informações a seguir a partir de seu roteador físico ou o administrador da rede no controle daquele roteador:

- Roteador ASN  
- Endereço IP do roteador  
- ASN para uso por componentes SDN (pode ser qualquer número de intervalo de ASN privado)

>[!NOTE]
>Quatro bytes ASNs não são suportados pelo SLB/Multiplexador. Você deve alocar dois bytes ASNs SLB/Multiplexador e o roteador wo que ele se conecta. Você pode usar 4 bytes ASNs em outro lugar em seu ambiente.  
  
Você ou o administrador de rede deve configurar o par de roteador BGP para aceitar conexões do ASN e o endereço IP ou o endereço de sub-rede da rede lógico trânsito seu gateway RAS e SLB/MUXes estão usando.
  
Para obter mais informações, consulte [Border Gateway Protocol & #40; BGP & #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).
  
## <a name="default-gateways"></a>Gateways padrão

Computadores que estão configurados para se conectar a várias redes, como os hosts físicos e máquinas virtuais de gateway só devem ter um gateway padrão configurado. O gateway padrão normalmente será configurado no adaptador usado para alcançar completamente com a Internet.

Para máquinas virtuais, use as seguintes regras para decidir qual rede para usar como o gateway padrão:

1. Use a rede de trânsito como o gateway padrão, se uma máquina virtual está conectada à rede de trânsito, ou se trata de hospedagem múltipla para o trânsito e qualquer outra rede.  
2. Use a rede de gerenciamento como o gateway padrão, se uma máquina virtual só é conectada à rede gerenciamento.  
3.  A rede de provedor de HNV nunca deve ser usada como um gateway padrão. As máquinas virtuais somente conectadas a essa rede será o SLB/MUXes e Gateways RAS.  
4.  Máquinas virtuais nunca será ser conectadas diretamente para as redes Storage1, Armazenamento2 do, VIP pública ou VIP particular.  
  
Para hosts Hyper-V e nós de armazenamento, use a rede de gerenciamento como o gateway padrão.  As redes de armazenamento nunca devem ter um gateway padrão atribuído.
  
## <a name="network-hardware"></a>Hardware de rede

Você pode usar as seções a seguir para planejar a implantação de hardware de rede.

### <a name="network-interface-cards-nics"></a>Placas de rede (NICs)

Para obter melhor desempenho, funcionalidades específicas são necessárias nas placas de interface de rede que você usa em seu hosts Hyper-V e hosts de armazenamento.  
 
Acesso direto de memória remoto (RDMA) é um kernel ignorar técnica que torna possível transferir grandes quantidades de dados sem envolver o host da CPU. Como o mecanismo DMA no adaptador de rede realiza a transferência, a CPU não é usada para a movimentação de memória.  Isso libera a CPU para executar outros trabalhos.  

Agrupamento Embedded switch (conjunto) é uma alternativa solução NIC agrupamento que você pode usar em ambientes que incluem o Hyper-V e a pilha de rede definidos Software (SDN) no Windows Server 2016. CONJUNTO integra algumas funcionalidades de agrupamento de NIC o Hyper-V Virtual Switch.

Para obter mais informações, consulte [remoto acesso direto à memória & #40; RDMA & #41; e alternar agrupamento Embedded & #40; Definir & #41;](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

Para levar em conta a sobrecarga de tráfego de rede virtual locatário causada por VXLAN ou NVGRE cabeçalhos de encapsulamento, o MTU da rede camada 2 fabric (comutadores e hosts) deve ser definido como maior ou igual a 1674 Bytes \ (incluindo headers\ Ethernet de camada 2). NICs que dão suporte a novos *EncapOverhead* palavra-chave avançadas do adaptador definirá o MTU automaticamente por meio do agente do Host do controlador de rede. NICs que não dão suporte a novos *EncapOverhead* palavra-chave é necessário definir o tamanho de MTU manualmente em cada host físico usando o *JumboPacket* \(or equivalent\) palavra-chave.

### <a name="switches"></a>Switches
  
Ao selecionar um comutador físico e um roteador para seu ambiente Certifique-se compatível com o seguinte conjunto de recursos.  

- Switchport MTU configurações \(required\)  
- MTU definido como > = 1674 Bytes \ (incluindo Header\ Ethernet L2)  
- L3 protocolos \(required\)  
- ECMP  
- BGP \(IETF RFC 4271\)\-based ECMP

Implementações devem dar suporte as instruções deve nos seguintes padrões IETF.

- RFC 2545: "BGP 4 protocolos extensões para IPv6 inter-Domain Routing"  
- RFC 4760: "extensões protocolos múltiplos de 4 BGP"  
- RFC 4893: "BGP suporte para AS quatro octet número espaço"  
- RFC 4456: "reflexão de rota BGP: uma alternativa para Full Mesh BGP interno (IBGP)"  
- RFC 4724: "normal reiniciar o mecanismo para BGP"  

Os seguintes protocolos de marcação são necessários.

- VLAN - isolamento de vários tipos de tráfego
- 802.1 Q tronco

Os itens a seguir fornecem controle do Link.

- Qualidade de serviço \ (necessário somente se usando RoCE\ PFC)
- Avançado tráfego seleção \(802.1Qaz\)
- Prioridade com base em fluxo de controle \ (802.1 p/Q e 802.1Qbb\)

Os itens a seguir fornecem disponibilidade e redundância.

- Alternar disponibilidade (obrigatória)
- Um roteador altamente disponível é necessária para executar as funções de gateway. Você pode fazer isso usando um roteador chassi várias switch\ ou tecnologias como VRRP.
        
Os itens a seguir fornecem recursos de gerenciamento.

**Monitoramento**

- SNMP v1 ou SNMP v2 (obrigatório se usando o controlador de rede para monitoramento de comutador físico)  
- SNMP MIBs \ (obrigatório se você estiver usando o controlador de rede para monitoring\ comutador físico)  
- MIB-II (RFC 1213), LLDP, Interface MIB \(RFC 2863\), IF-MIB, IP-MIB, IP-encaminhar-MIB, Q-PONTE-MIB, MIB PONTE, LLDB MIB, entidade MIB, IEEE8023-LATÊNCIA-MIB  
  
Os diagramas a seguir mostram uma configuração de quatro nós de amostra. Para fins de esclarecimento, do primeiro diagrama mostra apenas o controlador de rede, a segunda mostra o controlador de rede mais balanceador o software e o terceiro diagrama mostra o gateway, balanceador de software e o controlador de rede.  
  
VNICs e redes de armazenamento não são shonwn nesses diagramas. Se você pretende usar armazenamento com base em SMB, eles são necessários.    
  
A infraestrutura e o locatário máquinas virtuais pode ser redistribuídas em qualquer host de computação física (assumindo que a conectividade de rede correto existe para as redes de lógicas corretas).  
  
### <a name="network-controller-deployment"></a>Implantação de controlador de rede

Antes de implantar o controlador de rede, você deve revisar instalação e requisitos de software, bem como configurar grupos de segurança e registro DNS dinâmico. Para obter mais informações, consulte [instalação e requisitos de preparação para o controlador de rede implantar](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md).

A configuração é altamente disponível com três nós de controlador de rede configuradas em máquinas virtuais. Também mostrado é dois locatários com rede virtual do locatário 2 dividida em duas sub-redes virtuais para simular uma faixa de web e uma camada de banco de dados.  

![Planejamento de NC SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>Implantação balanceador de carga de software e controlador de rede

Para alta disponibilidade, há dois ou mais nós SLB/Multiplexador.
   
![Planejamento de NC SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)
  
### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Implantação de controlador de rede, Software balanceador e RAS Gateway

Há três máquinas virtuais de gateway; dois estão ativas e uma é redundante.

![Planejamento de NC SDN](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  
  
  
  
Para a automação da implantação baseada em TP5, Active Directory deve estar disponível e acessível dessas sub-redes. Para obter mais informações sobre o Active Directory, consulte [visão geral do Active Directory domínio serviços](https://technet.microsoft.com/en-us/library/mt703721.aspx).  
  
>[!IMPORTANT] 
>Se você implantar usando VMM, certifique-se de suas máquinas virtuais de infraestrutura (servidor VMM, AD/DNS, SQL Server, etc.) não são hospedados em qualquer um dos quatro hosts mostrados nos diagramas a.  
  
## <a name="switch-configuration-examples"></a>Exemplos de configuração do switch  
  
Para ajudar a configurar seu roteador ou comutador físico, um conjunto de arquivos de configuração de exemplo por uma variedade de modelos de switch e fornecedores estão disponíveis na [repositório Github de SDN Microsoft](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). Um arquivo Leiame detalhada e comandos de interface de linha de comando (CLI) testado para opções específicas são fornecidos.         
  
  
## <a name="compute"></a>Calcular  
Todos os hosts Hyper-V devem ter o Windows Server 2016 instalado, Hyper-V habilitados e um comutador virtual externo do Hyper-V criado com pelo menos um adaptador físico conectado à rede gerenciamento lógica. O host deve ser acessível por meio de um endereço IP de gerenciamento atribuído para o Host de gerenciamento vNIC.  
  
Qualquer tipo de armazenamento que é compatível com o Hyper-V, compartilhado ou local pode ser usado.   
  
> [!TIP]  
> É conveniente se você usar o mesmo nome para todos os seus comutadores virtuais, mas não é obrigatório. Se você planeja implantar com scripts, consulte o comentário associado a `vSwitchName`variável no arquivo config.psd1.  
  
**Requisitos de computação do host**  
A tabela a seguir mostra os requisitos mínimos de hardware e software para os quatro hosts físicos usados na implantação de exemplo.  
  
Host|Requisitos de hardware|Requisitos de software|  
--------|-------------------------|-------------------------  
|Host do Hyper-v físico|4 núcleos 2,66 GHz da CPU<br /><br />32 GB de RAM<br /><br />Espaço em disco 300 GB<br /><br />1 Gb/s (ou mais rápido) adaptador de rede física|Sistema operacional: Windows Server 2016<br /><br />Função Hyper-V instalada|  
  
  
**Requisitos de função de máquina virtual de infraestrutura SDN**  
  
Função|Requisitos de vCPU|Requisitos de memória|Requisitos de disco|  
--------|---------------------|-----------------------|---------------------  
|Controlador de rede (nó três)|4 vCPUs|4 GB min (recomenda-se 8 GB)|75 GB para a unidade do sistema operacional  
|SLB/Multiplexador (nó três)|8 vCPUs|8 GB recomendado|75 GB para a unidade do sistema operacional  
|Gateway RAS<br /><br />(único pool de gateways de nó três, dois ativos, um passivo)|8 vCPUs|8 GB recomendado|75 GB para a unidade do sistema operacional  
|Roteador RAS Gateway BGP para SLB/Multiplexador estranhas<br /><br />(como alternativa usar switch ToR como roteador BGP)|2 vCPUs|2 GB|75 GB para a unidade do sistema operacional|  
  
  
Se você usar VMM para implantação, recursos de máquina virtual de infraestrutura adicionais são necessários para VMM e outra infraestrutura SDN não. Para obter informações adicionais, consulte [mínimo recomendações de Hardware para o System Center Technical Preview.](https://technet.microsoft.com/library/dn997303.aspx)  
  
## <a name="extending-your-infrastructure"></a>Estendendo sua infraestrutura  
Os requisitos de dimensionamento e recursos para sua infraestrutura dependem as máquinas de virtuais de carga de trabalho de locatário que você planeja hospedar. A CPU, memória e requisitos de disco para as máquinas virtuais de infraestrutura (por exemplo: gateway de controlador, SLB, rede, etc.) estão listados na tabela anterior. Você pode adicionar mais dessas máquinas de virtuais de infraestrutura de dimensionar conforme necessário. No entanto, qualquer máquinas virtuais de locatário em execução nos hosts Hyper-V tem sua própria CPU, memória e requisitos de disco que você deve levar em consideração.   
  
Quando as máquinas de virtuais de carga de trabalho de locatário começam a consumir muitos recursos nos hosts Hyper-V físicos, você pode estender sua infraestrutura adicionando outros hosts físicos. Isso pode ser feito com o Gerenciador de máquina Virtual ou usando scripts do PowerShell (dependendo da maneira como você inicialmente implantados a infraestrutura) para criar novos recursos do servidor por meio do controlador de rede. Se você precisar adicionar outros endereços IP para a rede HNV provedor, você pode criar novos sub-redes lógicas (com correspondente Pools de IP) que podem usar os hosts.  
  
  
## <a name="see-also"></a>Consulte também  
[Instalação e requisitos de preparação para a implantação de controlador de rede](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[Software definidos rede & #40; SDN & #41;](../Software-Defined-Networking--SDN-.md)  
  


