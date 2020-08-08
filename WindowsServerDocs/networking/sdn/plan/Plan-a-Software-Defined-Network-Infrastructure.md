---
title: Planejar uma infraestrutura de rede definida pelo software
description: Este tópico fornece informações sobre como planejar sua implantação de infraestrutura de SDN (rede definida pelo software).
manager: grcusanz
ms.topic: get-started-article
ms.assetid: ea7e53c8-11ec-410b-b287-897c7aaafb13
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/10/2018
ms.openlocfilehash: 1930ee8d74a1aa99b5c94df19e572d382144e604
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996559"
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Planejar uma infraestrutura de rede definida pelo software

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Saiba mais sobre o planejamento de implantação para uma infraestrutura de rede definida pelo software, incluindo os pré-requisitos de hardware e software.


## <a name="prerequisites"></a>Pré-requisitos
Este tópico descreve uma série de pré-requisitos de hardware e software, incluindo:

-   **Grupos de segurança configurados, locais de arquivos de log e registro de DNS dinâmico** Você deve preparar o datacenter para a implantação do controlador de rede, o que exige um ou mais computadores ou VMs e um computador ou VM. Antes de implantar o controlador de rede, você deve configurar os grupos de segurança, os locais do arquivo de log (se necessário) e o registro de DNS dinâmico.  Se você não preparou seu datacenter para a implantação do controlador de rede, consulte [requisitos de instalação e preparação para implantar o controlador de rede](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md) para obter detalhes.

-   **Rede física**  Você precisa acessar seus dispositivos de rede física para configurar VLANs, roteamento, BGP, Data Center Bridging (ETS) se estiver usando uma tecnologia RDMA e a ponte do Data Center (PFC) se estiver usando uma tecnologia RDMA baseada em RoCE. Este tópico mostra a configuração manual do comutador, bem como o emparelhamento via protocolo BGP em comutadores/roteadores de camada 3, ou em uma máquina virtual RRAS (servidor de roteamento e acesso remoto).

-   **Hosts de computação física**  Esses hosts executam o Hyper-V e são necessários para hospedar máquinas virtuais de locatário e infraestrutura de SDN.  O hardware de rede específico é necessário nesses hosts para obter o melhor desempenho, que é descrito posteriormente na seção **hardware de rede** .


## <a name="physical-network-and-compute-host-configuration"></a>Configuração de host de computação e rede física

Cada host de computação física requer conectividade de rede por meio de um ou mais adaptadores de rede anexados a uma ou mais portas de comutador físico.  Uma [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN) de camada 2 dá suporte a redes divididas em vários segmentos de rede lógica.

>[!TIP]
>Use a VLAN 0 para redes lógicas no modo de acesso ou sem marcas.

>[!IMPORTANT]
>A rede de software definida pelo Windows Server 2016 dá suporte ao endereçamento IPv4 para o underlay e a sobreposição. Não há suporte para IPv6.

### <a name="logical-networks"></a>Logical networks

#### <a name="management-and-hnv-provider"></a>Gerenciamento e provedor de HNV

Todos os hosts de computação física devem acessar a rede lógica de gerenciamento e a rede lógica do provedor de HNV.  Para fins de planejamento de endereço IP, cada host de computação física deve ter pelo menos um endereço IP atribuído da rede lógica de gerenciamento. O controlador de rede requer um endereço IP reservado para servir como o endereço IP REST.

Um servidor DHCP pode atribuir automaticamente endereços IP para a rede de gerenciamento ou você pode atribuir manualmente o endereço IP estático. A pilha de SDN atribui automaticamente endereços IP para a rede lógica do provedor de HNV para os hosts individuais do Hyper-V de um pool de IPS especificado e gerenciado pelo controlador de rede.

>[!NOTE]
>O controlador de rede atribui um endereço IP do provedor de HNV a um host de computação física somente depois que o agente de host do controlador de rede recebe a política de rede para uma máquina virtual de locatário específica.


|                                                               Se...                                                               |                                                                                                                                                                          Então...                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                  As redes lógicas usam VLANs,                                                  |                                                                 o host de computação física deve se conectar a uma porta de comutador de tronco que tem acesso a essas VLANs. É importante observar que os adaptadores de rede física no host do computador não devem ter nenhuma filtragem de VLAN ativada.                                                                 |
|                Usando o conjunto de agrupamentos incorporados (SET) e ter vários membros da equipe NIC, como adaptadores de rede,                |                                                                                                                        Você deve conectar todos os membros da equipe NIC para esse host específico ao mesmo domínio de difusão de camada 2.                                                                                                                         |
| O host de computação física está executando máquinas virtuais de infraestrutura adicionais, como controlador de rede, SLB/MUX ou gateway, | esse host deve ter um endereço IP adicional atribuído da rede lógica de gerenciamento para cada uma das máquinas virtuais hospedadas.<p>Além disso, cada máquina virtual de infraestrutura SLB/MUX deve ter um endereço IP reservado para a rede lógica do provedor de HNV. A falha em ter um endereço IP reservado pode resultar em endereços IP duplicados em sua rede. |

---

Para obter informações sobre a HNV (virtualização de rede) do Hyper-V, que você pode usar para virtualizar redes em uma implantação do Microsoft SDN, consulte [virtualização de rede do Hyper-v](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).



#### <a name="gateways-and-the-software-load-balancer"></a>Gateways e o software Load Balancer

Redes lógicas adicionais precisam ser criadas e provisionadas para o uso de gateway e SLB. Certifique-se de obter os prefixos IP corretos, IDs de VLAN e endereços IP de gateway para essas redes.


|                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **Rede lógica de trânsito**   | O gateway RAS e o SLB/MUX usam a rede lógica de trânsito para trocar informações de emparelhamento de BGP e tráfego de locatário de Norte/Sul (externo interno). O tamanho dessa sub-rede normalmente será menor do que as outras. Somente os hosts de computação física que executam o gateway RAS ou máquinas virtuais SLB/MUX precisam ter conectividade com essa sub-rede com essas VLANs trunked e acessíveis nas portas do comutador às quais os adaptadores de rede dos hosts de computação estão conectados. Cada máquina virtual SLB/MUX ou gateway RAS é atribuída estaticamente a um endereço IP da rede lógica de trânsito. |
| **Rede lógica VIP pública**  |                                                                                                                             A rede lógica VIP pública é necessária para ter prefixos de sub-rede IP roteáveis fora do ambiente de nuvem (normalmente Internet roteável).  Esses serão os endereços IP de front-end usados por clientes externos para acessar recursos nas redes virtuais, incluindo o VIP de front-end para o Gateway site a site.                                                                                                                             |
| **Rede lógica VIP privada** |                                                                                                                                                                                       A rede lógica VIP privada não precisa ser roteável fora da nuvem, pois ela é usada para VIPs que são acessados somente de clientes de nuvem internos, como o SLB Manager ou serviços privados.                                                                                                                                                                                       |
|   **Rede lógica VIP GRE**   |                                                                                                                                           A rede de VIP do GRE é uma sub-rede que existe somente para definir VIPs atribuídos às máquinas virtuais de gateway que está sendo executado em sua malha SDN para um tipo de conexão GRE S2S. Essa rede não precisa ser pré-configurada em seus comutadores físicos ou no roteador nem precisa ter uma VLAN atribuída.                                                                                                                                            |

---


#### <a name="sample-network-topology"></a>Topologia de rede de exemplo
Altere os prefixos de sub-rede IP de exemplo e as IDs de VLAN para seu ambiente.


| **Nome da rede** |  **Sub-rede**  | **Mascara** | **ID de VLAN no caminhão** | **Gateway**  |                                                           **Reservas (exemplos)**                                                           |
|------------------|--------------|----------|----------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
|    Gerenciamento    | 10.184.108.0 |    24    |          7           | 10.184.108.1 | 10.184.108.1 – roteador 10.184.108.4-controlador de rede 10.184.108.10-computação host 110.184.108.11-Compute host 210.184.108. X – host de computação X |
|   Provedor de HNV   |  10.10.56.0  |    23    |          11          |  10.10.56.1  |                                                    10.10.56.1 – roteador 10.10.56.2-SLB/MUX1                                                     |
|     Trânsito      |  rede 10.10.10.0  |    24    |          10          |  10.10.10.1  |                                                               10.10.10.1 – roteador                                                               |
|    VIP público    |  41.40.40.0  |    27    |          NA          |  41.40.40.1  |                                    41.40.40.1 – roteador 41.40.40.2-SLB/MUX VIP 41.40.40.3-VIP VPN S2S IPSec                                    |
|   VIP privado    |  20.20.20.0  |    27    |          NA          |  20.20.20.1  |                                                        20.20.20.1 – GW padrão (roteador)                                                         |
|     VIP do GRE      |  31.30.30.0  |    24    |          NA          |  31.30.30.1  |                                                             31.30.30.1-GW padrão                                                             |

---

### <a name="logical-networks-required-for-rdma-based-storage"></a>Redes lógicas necessárias para o armazenamento baseado em RDMA

Se estiver usando o armazenamento baseado em RDMA, defina uma VLAN e uma sub-rede para cada adaptador físico (dois adaptadores por nó) em seus hosts de computação e armazenamento.

>[!IMPORTANT]
>Para que a qualidade de serviço (QoS) seja aplicada adequadamente, os comutadores físicos exigem uma VLAN marcada para o tráfego RDMA.

| **Nome da rede** |  **Sub-rede**  | **Mascara** | **ID de VLAN no caminhão** | **Gateway**  |                                                           **Reservas (exemplos)**                                                            |
|------------------|--------------|----------|----------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
|     Armazenamento1     |  10.60.36.0  |    25    |          8           |  10.60.36.1  |  10.60.36.1 – roteador<p>10.60.36. X – host de computação X<p>10.60.36. y-host de computação Y<p>10.60.36. V-cluster de computação<p>10.60.36. W-cluster de armazenamento  |
|     Storage2     | 10.60.36.128 |    25    |          9           | 10.60.36.129 | 10.60.36.129 – roteador<p>10.60.36. X – host de computação X<p>10.60.36. y-host de computação Y<p>10.60.36. V-cluster de computação<p>10.60.36. W-cluster de armazenamento |

---


## <a name="routing-infrastructure"></a>Infraestrutura de roteamento

Se você estiver implantando sua infraestrutura de SDN usando scripts, as sub-redes de gerenciamento, provedor HNV, trânsito e VIP deverão ser roteáveis entre si na rede física.

As informações de roteamento \( , por exemplo, o próximo salto \) para as sub-redes VIP são anunciadas pelos gateways SLB/MUX e RAS na rede física usando o emparelhamento via protocolo BGP interno. As redes lógicas VIP não têm uma VLAN atribuída e não são pré-configuradas na opção de camada 2 (por exemplo, o comutador Top-of-rack).

Você precisa criar um par de BGP no roteador que é usado pela sua infraestrutura de SDN para receber rotas para as redes lógicas VIP anunciadas pelos gateways SLB/MUXes e RAS. O emparelhamento via protocolo BGP só precisa ocorrer de uma maneira (do SLB/MUX ou do gateway de RAS para o par de BGP externo).  Acima da primeira camada de roteamento, você pode usar rotas estáticas ou outro protocolo de roteamento dinâmico, como o OSPF, no entanto, como mencionado anteriormente, o prefixo de sub-rede IP para as redes lógicas VIP precisa ser roteável da rede física para o par de BGP externo.

O emparelhamento via protocolo BGP normalmente é configurado em um comutador ou roteador gerenciado como parte da infraestrutura de rede. O par de BGP também pode ser configurado em um Windows Server com a função de RAS (servidor de acesso remoto) instalada em um modo somente roteamento. Esse par de roteador BGP na infraestrutura de rede deve ser configurado para ter seu próprio ASN e permitir o emparelhamento de um ASN atribuído aos componentes SDN \( SLB/MUX e gateways RAS \) . Você deve obter as seguintes informações do roteador físico ou do administrador de rede no controle desse roteador:

- ASN do roteador
- Endereço IP do roteador
- ASN para uso por componentes de SDN (pode ser qualquer número de as a partir do intervalo ASN privado)

>[!NOTE]
>Não há suporte para ASNs de quatro bytes pelo SLB/MUX. Você deve alocar dois bytes de ASNs para o SLB/MUX e o roteador Wo que ele conecta. Você pode usar o ASNs de 4 bytes em outro lugar em seu ambiente.

Você ou seu administrador de rede deve configurar o par de roteador BGP para aceitar conexões do ASN e endereço IP ou endereço de sub-rede da rede lógica de trânsito que o seu gateway RAS e SLB/MUXes estão usando.

Para obter mais informações, consulte [Border Gateway Protocol (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

## <a name="default-gateways"></a>Gateways padrão
Os computadores configurados para se conectar a várias redes, como hosts físicos e máquinas virtuais de gateway, devem ter apenas um gateway padrão configurado. Configure o gateway padrão no adaptador usado para acessar a Internet.

Para máquinas virtuais, siga estas regras para decidir qual rede usar como o gateway padrão:

1. Use a rede lógica de trânsito como o gateway padrão se uma máquina virtual se conectar à rede de trânsito ou se for de hospedagem múltipla para a rede de trânsito ou para qualquer outra rede.
2. Use a rede de gerenciamento como o gateway padrão se uma máquina virtual se conectar somente à rede de gerenciamento.
3. Use a rede do provedor HNV para os gateways SLB/MUXes e RAS. Não use a rede do provedor HNV como um gateway padrão.
4. Não conecte máquinas virtuais diretamente às redes Storage1, Storage2, VIP público ou VIP privado.

Para hosts e nós de armazenamento do Hyper-V, use a rede de gerenciamento como o gateway padrão.  As redes de armazenamento nunca devem ter um gateway padrão atribuído.


## <a name="network-hardware"></a>Hardware de rede

Você pode usar as seções a seguir para planejar a implantação de hardware de rede.

### <a name="network-interface-cards-nics"></a>Placas de interface de rede (NICs)

As placas de interface de rede (NICs) usadas em seus hosts Hyper-V e hosts de armazenamento exigem recursos específicos para obter o melhor desempenho.

O RDMA (acesso remoto direto à memória) é uma técnica de bypass de kernel que torna possível transferir grandes quantidades de dados sem usar a CPU do host, o que libera a CPU para executar outro trabalho.

Mudar o agrupamento incorporado (conjunto) é uma solução de agrupamento NIC alternativa que você pode usar em ambientes que incluem o Hyper-V e a pilha de SDN (rede definida pelo software) no Windows Server 2016. O conjunto integra algumas funcionalidades de agrupamento NIC ao comutador virtual Hyper-V.

Para obter mais informações, consulte [acesso remoto direto à memória (RDMA) e Comutador incorporado (Set)](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

Para considerar a sobrecarga no tráfego de rede virtual de locatário causado por cabeçalhos de encapsulamento VXLAN ou NVGRE, o MTU da rede de malha de camada 2 (comutadores e hosts) deve ser definido como maior ou igual a 1674 bytes, \( incluindo cabeçalhos de Ethernet de camada 2 \) .

As NICs que dão suporte à nova palavra-chave do adaptador avançado *EncapOverhead* definem o MTU automaticamente por meio do agente de host do controlador de rede. As NICs que não dão suporte à nova palavra-chave *EncapOverhead* precisam definir o tamanho de MTU manualmente em cada host físico usando o *JumboPacket* \( ou a \) palavra-chave equivalente.


### <a name="switches"></a>Opções

Ao selecionar um comutador físico e um roteador para o seu ambiente, verifique se ele dá suporte ao seguinte conjunto de recursos:

- Configurações de MTU de switchport \( necessárias\)
- MTU definido como >= 1674 bytes \( , incluindo o cabeçalho L2-Ethernet\)
- Protocolos L3 \( necessários\)
- ECMP
- \( \) \- ECMP baseada em BGP IETF RFC 4271

As implementações devem dar suporte às instruções deve nos seguintes padrões de IETF.

- RFC 2545: "extensões do protocolo BGP-4 para roteamento entre domínios IPv6"
- RFC 4760: "extensões de multiprotocolo para BGP-4"
- RFC 4893: "suporte de BGP para quatro octetos como espaço numérico"
- RFC 4456: "reflexão de rota BGP: uma alternativa ao BGP interno de malha completa (IBGP)"
- RFC 4724: "mecanismo de reinicialização normal para BGP"

Os seguintes protocolos de marcação são necessários.

- Isolamento de VLAN de vários tipos de tráfego
- tronco 802.1 q

Os itens a seguir fornecem controle de link.

- A qualidade do \( PFC de serviço só é necessária se estiver usando RoCE\)
- Seleção de tráfego aprimorada \( 802.1 Qaz\)
- Controle de fluxo baseado em prioridade \( 802.1 p/Q e 802.1 QBB\)

Os itens a seguir fornecem disponibilidade e redundância.

- Disponibilidade do comutador (obrigatório)
- Um roteador altamente disponível é necessário para executar funções de gateway. Você pode fazer isso usando um comutador de vários chassis \ roteador ou tecnologias como VRRP.

Os itens a seguir fornecem recursos de gerenciamento.

**Monitoring**

- SNMP v1 ou SNMP v2 (necessário se estiver usando o controlador de rede para o monitoramento de comutador físico)
- MIBs \( de SNMP são necessárias se você estiver usando o controlador de rede para monitoramento de comutador físico\)
- MIB-II (RFC 1213), LLDP, MIB de interface \( RFC 2863 \) , IF-MIB, IP-MIB, IP-forward-MIB, p-Bridge-MIB, Bridge-MIB, LLDB-MIB, Entity-MIB, IEEE8023-lag-MIB

Os diagramas a seguir mostram um exemplo de configuração de quatro nós. Para fins de clareza, o primeiro diagrama mostra apenas o controlador de rede, o segundo mostra o controlador de rede mais o balanceador de carga de software e o terceiro diagrama mostra o controlador de rede, o balanceador de carga de software e o gateway.

Esses diagramas não mostram redes de armazenamento e vNICs. Se você planeja usar o armazenamento baseado em SMB, eles são necessários.

As máquinas virtuais de locatário e de infraestrutura podem ser redistribuídas em qualquer host de computação física (supondo que exista a conectividade de rede correta para as redes lógicas corretas).



## <a name="switch-configuration-examples"></a>Alternar exemplos de configuração

Para ajudar a configurar seu comutador físico ou roteador, um conjunto de arquivos de configuração de exemplo para uma variedade de modelos de switch e fornecedores está disponível no [repositório GitHub do Microsoft Sdn](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). São fornecidos um Leiame detalhado e comandos de CLI (interface de linha de comando) testados para opções específicas.


## <a name="compute"></a>Computação
Todos os hosts Hyper-V devem ter o Windows Server 2016 instalado, o Hyper-V habilitado e um comutador virtual Hyper-V externo criado com pelo menos um adaptador físico conectado à rede lógica de gerenciamento. O host deve estar acessível por meio de um endereço IP de gerenciamento atribuído ao host de gerenciamento vNIC.

Qualquer tipo de armazenamento compatível com o Hyper-V, compartilhado ou local pode ser usado.

> [!TIP]
> É conveniente se você usa o mesmo nome para todos os seus comutadores virtuais, mas não é obrigatório. Se você planeja implantar com scripts, consulte o comentário associado à `vSwitchName` variável no arquivo config.psd1.

**Requisitos de computação do host** A tabela a seguir mostra os requisitos mínimos de hardware e software para os quatro hosts físicos usados na implantação de exemplo.

Host|Requisitos de hardware|Requisitos de software|
--------|-------------------------|-------------------------
|Host do Hyper-v físico|CPU 4-core de 2,66 GHz<p>32 GB de RAM<p>300 GB de espaço em disco<p>1 GB/s (ou mais rápido) adaptador de rede física|Sistema operacional: Windows Server 2016<p>Função do Hyper-V instalada|


**Requisitos da função de máquina virtual de infraestrutura SDN**

Função|requisitos do vCPU|Requisitos de memória|Requisitos de disco|
--------|---------------------|-----------------------|---------------------
|Controlador de rede (três nós)|4 vCPUs|4 GB min (8 GB recomendado)|75 GB para a unidade do sistema operacional
|SLB/MUX (três nós)|8 vCPUs|8 GB recomendados|75 GB para a unidade do sistema operacional
|Gateway de RAS<p>(pool único de três gateways de nó, dois ativos, um passivo)|8 vCPUs|8 GB recomendados|75 GB para a unidade do sistema operacional
|Roteador BGP de gateway RAS para emparelhamento de SLB/MUX<p>(como alternativa, use a opção ToR como roteador BGP)|2 vCPUs|2 GB|75 GB para a unidade do sistema operacional|


Se você usar o VMM para implantação, recursos adicionais de máquina virtual de infraestrutura serão necessários para o VMM e outra infraestrutura não SDN. Para obter informações adicionais, consulte [recomendações de hardware mínimas para o System Center Technical Preview.](/system-center/)

## <a name="extending-your-infrastructure"></a>Estendendo sua infraestrutura
Os requisitos de dimensionamento e de recursos para sua infraestrutura dependem das máquinas virtuais de carga de trabalho do locatário que você planeja hospedar. Os requisitos de CPU, memória e disco para as máquinas virtuais de infraestrutura (por exemplo: controlador de rede, SLB, gateway, etc.) são listados na tabela anterior. Você pode adicionar mais dessas máquinas virtuais de infraestrutura para escalar horizontalmente, conforme necessário. No entanto, todas as máquinas virtuais de locatário em execução nos hosts Hyper-V têm seus próprios requisitos de CPU, memória e disco que você deve considerar.

Quando as máquinas virtuais de carga de trabalho do locatário começam a consumir muitos recursos nos hosts físicos do Hyper-V, você pode estender sua infraestrutura adicionando hosts físicos adicionais. Isso pode ser feito com Virtual Machine Manager ou usando scripts do PowerShell (dependendo de como você implantou inicialmente a infraestrutura) para criar novos recursos de servidor por meio do controlador de rede. Se precisar adicionar outros endereços IP para a rede do provedor de HNV, você poderá criar novas sub-redes lógicas (com os pools de IP correspondentes) que os hosts podem usar.


## <a name="see-also"></a>Consulte Também
[Requisitos de instalação e preparação para a implantação do controlador](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md) 
 de rede [Rede definida pelo Software &#40;SDN&#41;](../software-defined-networking.md)