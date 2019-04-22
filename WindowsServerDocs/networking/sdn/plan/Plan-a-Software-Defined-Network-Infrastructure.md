---
title: Planejar uma infraestrutura de rede definida pelo software
description: Este tópico fornece informações sobre como planejar a implantação de sua infra-estrutura de rede definida pelo Software (SDN).
manager: dougkim
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
ms.date: 08/10/2018
ms.openlocfilehash: e3f7f2b6e2f815ec1924b4d476d5e3fd3ab181ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821057"
---
# <a name="plan-a-software-defined-network-infrastructure"></a>Planejar uma infraestrutura de rede definida pelo software

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Saiba mais sobre o planejamento de implantação de uma infraestrutura de rede definida por Software, incluindo os pré-requisitos de hardware e software. 


## <a name="prerequisites"></a>Pré-requisitos
Este tópico descreve alguns dos pré-requisitos de hardware e software, incluindo:

-   **Configurado grupos de segurança, os locais de arquivo de log e registro DNS dinâmico** você deve preparar o seu datacenter para a implantação do controlador de rede, o que exige um ou mais computadores ou máquinas virtuais e um computador ou VM. Antes de implantar o controlador de rede, você deve configurar os grupos de segurança, locais de arquivo de log (se necessário) e o registro DNS dinâmico.  Se seu data center não está preparado para implantação do controlador de rede, consulte [instalação e requisitos de preparação para implantação de controlador de rede](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md) para obter detalhes.

-   **Rede física** precisam de acesso para seus dispositivos de rede física para configurar VLANs, roteamento, BGP, Data Center Bridging (ETS) se usando uma tecnologia RDMA, e o Data Center Bridging (PFC) se usando um RoCE com base em tecnologia RDMA. Este tópico mostra a configuração manual de comutador, bem como emparelhamento via protocolo BGP em comutadores de camada 3 / roteadores ou uma máquina virtual de roteamento e o servidor de acesso remoto (RRAS).   

-   **Hosts de computação física** esses hosts executam o Hyper-V e são necessários para hospedar SDN infraestrutura e locatário máquinas virtuais.  Hardware de rede específica é necessária nesses hosts para melhor desempenho, que é descrito posteriormente na **hardware de rede** seção.  
      
  
## <a name="physical-network-and-compute-host-configuration"></a>Configuração de host de computação e rede física

Cada host de computação física requer conectividade de rede por meio de um ou mais adaptadores de rede anexado a uma porta de comutador físico (s).  Camada 2 [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN) dá suporte a redes divididos em vários segmentos de rede lógica. 

>[!TIP]
>Use VLAN 0 para redes lógicas no modo de acesso ou sem marcas. 

>[!IMPORTANT]
>Windows Server 2016 rede definida pelo Software dá suporte a endereçamento IPv4 para a base e a sobreposição. IPv6 sem suporte.
  
### <a name="logical-networks"></a>Redes lógicas

#### <a name="management-and-hnv-provider"></a>Provedor de HNV e gerenciamento 

Todos os hosts de computação física devem acessar a rede lógica de gerenciamento e a rede lógica do provedor de HNV.  Para fins de planejamento de endereço de IP, cada host de computação física deve ter pelo menos um endereço IP atribuído da rede lógica de gerenciamento. O controlador de rede requer um endereço IP reservado para servir como o endereço IP de REST. 

Um servidor DHCP pode atribuir automaticamente endereços IP para a rede de gerenciamento, ou você pode atribuir manualmente o endereço IP estático. A pilha de SDN atribui automaticamente endereços IP para o provedor de HNV a rede lógica para os hosts do Hyper-V individuais de um Pool IP especificado por meio e gerenciados pelo controlador de rede. 

>[!NOTE]
>O controlador de rede atribui um endereço IP do provedor de HNV para um host de computação física somente depois que o agente de Host do controlador de rede recebe a política de rede para uma máquina virtual de locatário específico. 


|Se...  |Então...  |
|---------|---------|
|As redes lógicas usam VLANs,     |o host de computação física deve se conectar à porta de tronco de comutador que tenha acesso a essas VLANs. É importante observar que os adaptadores de rede física no host do computador não devem ter qualquer VLAN filtragem ativada.          |
|Usando Switched-Embedded Teaming (SET) e ter vários membros da equipe NIC, como adaptadores de rede     |Você deve conectar todos os membros da equipe NIC para que o host específico no mesmo domínio de difusão de camada 2.         |
|O host de computação física está executando máquinas virtuais de infraestrutura adicional, como o controlador de rede, o SLB/MUX ou o Gateway,  |Esse host deve ter um endereço IP adicional atribuído da rede lógica de gerenciamento para cada uma das máquinas virtuais hospedadas.<p>Além disso, cada máquina de virtual SLB/MUX infraestrutura deve ter um endereço IP reservado para a rede lógica do provedor de HNV. Se não houver um endereço IP reservado pode resultar em endereços IP duplicados na sua rede.  |
---

Para obter informações sobre o Hyper-V rede HNV (virtualização), que você pode usar a virtualização de redes em uma implantação de SDN Microsoft, consulte [virtualização de rede do Hyper-V](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  
  

  
#### <a name="gateways-and-the-software-load-balancer"></a>Gateways e o balanceador de carga de Software
  
Redes lógicas adicionais precisam ser criado e provisionado para o gateway e o uso SLB. Certifique-se obter os prefixos IP corretos, IDs de VLAN e endereços IP do gateway para essas redes.


|  |  |
|---------|---------|
|**Rede lógica de trânsito**     |O Gateway de RAS e o SLB/MUX usam a rede lógica de trânsito para trocar informações sobre o emparelhamento via protocolo BGP e o tráfego de locatário (externo-interno) do Norte/Sul. O tamanho dessa sub-rede normalmente será menor do que os outros. Somente os hosts de computação física que executam o Gateway de RAS ou máquinas virtuais SLB/MUX precisam ter conectividade com essa sub-rede com essas VLANs, troncos e acessíveis nas portas do comutador ao qual os adaptadores de rede dos hosts de computação estão conectados. Cada SLB/MUX ou máquina virtual de Gateway de RAS estaticamente é atribuída um endereço IP da rede lógica de trânsito.         |
|**Rede lógica VIP pública**     |A rede lógica VIP pública deve ter os prefixos de sub-rede IP que são roteáveis fora do ambiente de nuvem (normalmente roteável da Internet).  Esses serão os endereços IP front-end usados por clientes externos para acessar os recursos nas redes virtuais, incluindo o VIP de front-end do gateway Site a site.         |
|**Rede lógica VIP privada**     |A rede lógica VIP privada não é necessária ser roteável fora da nuvem, pois ele é usado para VIPs somente são acessados de clientes de nuvem interna, como o SLB gereciador ou serviços privados.         |
|**Rede lógica VIP do GRE**     |A rede VIP do GRE é uma sub-rede que existe somente para definir VIPs atribuídos às máquinas virtuais de gateway em execução em sua malha SDN para um tipo de conexão GRE S2S. Esta rede não precisa ser pré-configurada em seus comutadores físicos ou o roteador e não precisam de uma VLAN atribuída.            |
---


#### <a name="sample-network-topology"></a>Topologia de rede de exemplo
Altere as IDs de VLAN e prefixos de sub-rede IP de exemplo para o seu ambiente. 

| **Nome de rede** | **Subrede** | **Máscara** | **ID de VLAN no caminhão** | **Gateway** | **Reservas (exemplos)** |
| --- | --- | --- | --- | --- | --- |
| Management | 10.184.108.0 | 24 | 7 | 10.184.108.1 | 10.184.108.1 – computação de rede Controller10.184.108.10 - host de computação 110.184.108.11 - Router10.184.108.4 - hospedar 210.184.108.X - host de computação X |
| Provedor de HNV | 10.10.56.0 | 23 | 11 | 10.10.56.1 | 10.10.56.1 – Router10.10.56.2 - SLB/MUX1   |
| Trânsito | 10.10.10.0 | 24 | 10 | 10.10.10.1 | 10.10.10.1 – roteador |
| VIP público | 41.40.40.0 | 27 | N/D | 41.40.40.1 | 41.40.40.1 – Router41.40.40.2 SLB/MUX VIP41.40.40.3 - IPSec S2S VPN VIP   |
| VIP privada | 20.20.20.0 | 27 | N/D | 20.20.20.1 | 20.20.20.1 - GW padrão (roteador)   |
| VIP DO GRE | 31.30.30.0 | 24 | N/D | 31.30.30.1 | 31.30.30.1 - GW padrão |
---
  
### <a name="logical-networks-required-for-rdma-based-storage"></a>Redes lógicas necessárias para armazenamento baseado em RDMA  
  
Se usar o armazenamento com base em RDMA, defina uma VLAN e sub-rede para cada adaptador físico (dois adaptadores por nó) em seus hosts de computação e armazenamento.  

>[!IMPORTANT]
>Para qualidade de serviço (QoS) a serem aplicadas adequadamente, comutadores físicos exigem uma VLAN com marcas de formatação para o tráfego RDMA.

| **Nome de rede** | **Subrede** | **Máscara** | **ID de VLAN no caminhão** | **Gateway** | **Reservas (exemplos)** |
| --- | --- | --- | --- | --- | --- |
| storage1 | 10.60.36.0 | 25 | 8 | 10.60.36.1 | 10.60.36.1 – roteador<p>10.60.36.X - host de computação X<p>10.60.36.Y - Y de host de computação<p>10.60.36.V - cluster de computação<p>10.60.36.W - cluster de armazenamento |
| storage2 | 10.60.36.128 | 25 | 9 | 10.60.36.129 | 10.60.36.129 – roteador<p>10.60.36.X - host de computação X<p>10.60.36.Y - Y de host de computação<p>10.60.36.V - cluster de computação<p>10.60.36.W - cluster de armazenamento   |
---

 
## <a name="routing-infrastructure"></a>Infraestrutura de roteamento  
  
Se você estiver implantando sua infraestrutura SDN usando scripts, as sub-redes de gerenciamento, o provedor de HNV, o trânsito e o VIP devem ser roteáveis uns aos outros na rede física.     
  
Informações de roteamento \(por exemplo, próximo salto\) para o VIP sub-redes é anunciado pelo SLB/MUX e Gateways de RAS para a rede física usando o emparelhamento de BGP interno. Redes lógicas VIP não têm uma VLAN atribuída e não é pré-configurada no comutador de camada 2 (por exemplo, o comutador Top-of-Rack).  
  
Você precisará criar um par de BGP no roteador que é usado pela sua infraestrutura SDN para receber as rotas para as redes lógicas de VIP anunciadas pelo SLB/MUXes e Gateways de RAS. Emparelhamento via protocolo BGP só precisa ocorrer unidirecional (do SLB/MUX ou Gateway de RAS para o par de BGP externo).  Acima da primeira camada de roteamento que você pode usar rotas estáticas ou outro protocolo de roteamento dinâmico como OSPF, no entanto, conforme mencionado anteriormente, o prefixo de sub-rede IP para redes lógicas VIP precisa ser roteável da rede física para o par BGP externo.   
  
Emparelhamento via protocolo BGP normalmente é configurado em um switch gerenciado ou um roteador como parte da infraestrutura de rede. O par de BGP também pode ser configurado em um servidor Windows com a função de servidor de acesso remoto (RAS) instalada em um modo somente roteamento. Esse par de roteador BGP na infraestrutura de rede deve ser configurado para ter seu próprio ASN e permitir emparelhamento de um ASN que é atribuído para os componentes SDN \(SLB/MUX e Gateways de RAS\). Você deve obter as informações a seguir de seu roteador físico ou do administrador de rede no controle do que o roteador:

- Router ASN  
- Endereço IP do roteador  
- ASN para uso por componentes de SDN (pode ser qualquer número do intervalo ASN privado)

>[!NOTE]
>ASNs de quatro bytes não são compatíveis com o SLB/MUX. Você deve alocar ASNs de dois bytes para o SLB/MUX e o roteador wo que ele se conecta. Você pode usar ASNs de 4 bytes em outro lugar no seu ambiente.  
  
Você ou o administrador de rede deve configurar o par de roteador BGP para aceitar conexões do ASN e o endereço IP ou o endereço de sub-rede da rede lógica trânsito seu gateway de RAS e MUXes do SLB/estão usando.
  
Para obter mais informações, consulte [Border Gateway Protocol (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).
  
## <a name="default-gateways"></a>Gateways padrão
As máquinas que são configuradas para se conectar a várias redes, como os hosts físicos e máquinas virtuais de gateway só devem ter um gateway padrão configurado. Configure o gateway padrão no adaptador usado para acessar a Internet.

Para máquinas virtuais, siga estas regras para decidir qual rede a ser usado como o gateway padrão:

1. Use a rede lógica de trânsito como o gateway padrão, se uma máquina virtual se conecta à rede de trânsito, ou se for multihomed para a rede de trânsito ou qualquer outra rede.
2. Use a rede de gerenciamento como o gateway padrão, se uma máquina virtual apenas se conecta à rede de gerenciamento. 
3. Use a rede de provedor de HNV para/de MUXes SLB e Gateways de RAS. Não use a rede do provedor de HNV como um gateway padrão. 
4. Não conecte máquinas virtuais diretamente para as redes Storage1, Storage2, VIP público ou VIP privada.

Para hosts Hyper-V e nós de armazenamento, use a rede de gerenciamento como o gateway padrão.  As redes de armazenamento nunca devem ter um gateway padrão atribuído.

  
## <a name="network-hardware"></a>Hardware de rede

Você pode usar as seções a seguir para planejar a implantação de hardware de rede.

### <a name="network-interface-cards-nics"></a>Placas de interface de rede (NICs)

As placas de interface de rede (NICs) usadas em seus hosts Hyper-V e hosts de armazenamento exigem recursos específicos para obter o melhor desempenho. 

Direto memória RDMA (acesso remoto) é um kernel ignorar a técnica que torna possível transferir grandes quantidades de dados sem o uso de CPU host, que libera a CPU para executar outro trabalho. 

Switch Embedded Teaming (SET) é uma alternativa na solução agrupamento NIC que você pode usar em ambientes que incluem o Hyper-V e a pilha de Software Defined Networking (SDN) no Windows Server 2016. CONJUNTO integra algumas funcionalidades de agrupamento NIC no comutador Virtual do Hyper-V. 

Para obter mais informações, consulte [acesso direto à memória remoto (RDMA) e Switch Embedded Teaming (SET)](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).   

Para justificar a sobrecarga no tráfego de rede virtual do locatário causado por cabeçalhos de encapsulamento VXLAN ou NVGRE, a MTU da rede de malha camada-2 (comutadores e hosts) deve ser definida como maior que ou igual a 1674 Bytes \(incluindo Ethernet de camada 2 cabeçalhos\). 

As NICs que dão suporte a novos *EncapOverhead* palavra-chave do adaptador avançada define a MTU automaticamente por meio do agente de Host do controlador de rede. As NICs que não dão suporte a novos *EncapOverhead* palavra-chave é necessário definir o tamanho de MTU manualmente em cada host físico usando o *JumboPacket* \(ou equivalente\) palavra-chave. 


### <a name="switches"></a>Comutadores
  
Ao selecionar um comutador físico e um roteador para o seu ambiente, certifique-se de dá suporte ao seguinte conjunto de recursos:  

- As configurações de MTU Switchport \(necessária\)  
- MTU definido como > = 1674 Bytes \(incluindo o cabeçalho de Ethernet de L2\)  
- Protocolos de L3 \(necessária\)  
- ECMP  
- BGP \(IETF RFC 4271\)\-com base em ECMP

Implementações devem dar suporte as instruções deve nos seguintes padrões IETF.

- RFC 2545: "Extensões Multiprotocol BGP-4 para o roteamento do IPv6 entre domínios"  
- RFC 4760: "Extensões de protocolos múltiplos para BGP-4"  
- RFC 4893: "Número de suporte de protocolo BGP para AS quatro octetos espaço"  
- RFC 4456: "Rota BGP reflexão: Uma alternativa à malha completa de BGP interno (IBGP)"  
- RFC 4724: "Mecanismo de reinício normal para BGP"  

Protocolos de marcação a seguir são necessários.

- VLAN - o isolamento de vários tipos de tráfego
- 802.1Q tronco

Os itens a seguir fornecem controle de Link.

- Qualidade de serviço \(PFC apenas necessário se estiver usando RoCE\)
- Aprimorada a seleção de tráfego \(802.1Qaz\)
- Prioridade baseada em controle de fluxo \(802.1 p/Q e 802.1Qbb\)

Os itens a seguir fornecem redundância e disponibilidade.

- Disponibilidade do comutador (obrigatória)
- Um roteador de alta disponibilidade é necessário para executar funções de gateway. Você pode fazer isso usando um roteador de chassi várias switch\ ou tecnologias, como VRRP.
        
Os itens a seguir oferecem recursos de gerenciamento.

**Monitoramento**

- SNMP v1 ou v2 SNMP (necessário se estiver usando o controlador de rede para o monitoramento de comutador físico)  
- SNMP MIBs \(necessária se você estiver usando o controlador de rede para o monitoramento de comutador físico\)  
- MIB-II (RFC 1213) LLDP, Interface MIB \(RFC 2863\), IF-MIB, IP MIB, encaminhamento de IP de MIB, Q-PONTE-MIB, PONTE MIB, LLDB MIB, entidade-MIB, IEEE8023 de LATÊNCIA de MIB  
  
Os diagramas a seguir mostram um exemplo de configuração de quatro nós. Para fins de clareza, o primeiro diagrama mostra apenas o controlador de rede, o segundo mostra o controlador de rede e o balanceador de carga de software e o terceiro diagrama mostra o controlador de rede, balanceador de carga de software e o gateway.  
  
Esses diagramas não mostram as vNICs e as redes de armazenamento. Se você planeja usar o armazenamento baseado em SMB, eles são necessários.
  
Máquinas virtuais de infraestrutura e de locatário podem ser redistribuídas em qualquer host de computação física (supondo que a conectividade de rede correto existe para as redes lógicas corretas).  
  

  
## <a name="switch-configuration-examples"></a>Exemplos de configuração de comutador  
  
Para ajudar a configurar seu roteador ou comutador físico, um conjunto de arquivos de configuração de exemplo para uma variedade de modelos de switch e fornecedores estão disponíveis na [repositório Github Microsoft SDN](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples). Um arquivo Leiame detalhado e comandos de interface de linha de comando (CLI) testadas para opções específicas são fornecidos.         
  
  
## <a name="compute"></a>Computação  
Todos os hosts do Hyper-V devem ter o Windows Server 2016 instalado, o Hyper-V habilitado e um comutador virtual externo do Hyper-V criado com pelo menos um adaptador físico conectado à rede lógica de gerenciamento. O host deve ser acessível por meio de um endereço IP de gerenciamento atribuído para a vNIC do Host de gerenciamento.  
  
Qualquer tipo de armazenamento que é compatível com o Hyper-V, local ou compartilhado pode ser usado.   
  
> [!TIP]  
> É conveniente se você usar o mesmo nome para todos os seus comutadores virtuais, mas não é obrigatório. Se você planeja implantar com scripts, consulte o comentário associado com o `vSwitchName` variável no arquivo config.psd1.  
  
**Requisitos de computação do host**  
A tabela a seguir mostra os requisitos mínimos de hardware e software para quatro hosts físicos usados na implantação de exemplo.  
  
Host|Requisitos de Hardware|Requisitos de Software|  
--------|-------------------------|-------------------------  
|Host físico do Hyper-v|CPU 4-core de 2,66 GHz<br /><br />32 GB de RAM<br /><br />Espaço em disco de 300 GB<br /><br />1 Gb/s (ou mais rápido) adaptador de rede física|SISTEMA OPERACIONAL: Windows Server 2016<br /><br />Função do Hyper-V instalada|  
  
  
**Requisitos de função de máquina virtual de infraestrutura SDN**  
  
Função|requisitos de vCPU|Requisitos de memória|Requisitos de disco|  
--------|---------------------|-----------------------|---------------------  
|Controlador de rede (três nós)|4 vCPUs|Mínimo de 4 GB (8 GB recomendado)|75 GB para a unidade do sistema operacional  
|SLB/MUX (três nós)|8 vCPUs|8 GB recomendado|75 GB para a unidade do sistema operacional  
|Gateway de RAS<br /><br />(único pool de gateways de três nós, dois ativos, um passivo)|8 vCPUs|8 GB recomendado|75 GB para a unidade do sistema operacional  
|Roteador BGP do Gateway de RAS para o emparelhamento do SLB/MUX<br /><br />(como alternativa, usar o comutador ToR como roteador BGP)|2 vCPUs|2 GB|75 GB para a unidade do sistema operacional|  
  
  
Se você usar o VMM para a implantação, recursos de máquina virtual de infraestrutura adicionais são necessários para o VMM e outra infraestrutura de SDN não. Para obter mais informações, consulte [recomendações mínimas de Hardware do System Center Technical Preview.](https://technet.microsoft.com/library/dn997303.aspx)  
  
## <a name="extending-your-infrastructure"></a>Estendendo sua infraestrutura  
Os requisitos de dimensionamento e recursos para sua infraestrutura são dependentes de máquinas de virtuais de carga de trabalho do locatário que você planeja hospedar. A CPU, memória e os requisitos de disco para as máquinas virtuais de infraestrutura (por exemplo: gateway de controlador, o SLB, rede, etc.) são listados na tabela anterior. Você pode adicionar mais dessas máquinas virtuais de infraestrutura para escalar horizontalmente conforme necessário. No entanto, quaisquer máquinas virtuais de locatário em execução em hosts do Hyper-V têm seus próprios CPU, memória e os requisitos de disco que você deve considerar.   
  
Quando as máquinas virtuais da carga de trabalho do locatário começar a consumir muitos recursos nos hosts do Hyper-V físicos, você pode estender sua infraestrutura com a adição de outros hosts físicos. Isso pode ser feito com o Virtual Machine Manager ou usando scripts do PowerShell (dependendo de como você inicialmente implantou a infraestrutura) para criar novos recursos de servidor por meio do controlador de rede. Se você precisar adicionar mais endereços IP para a rede de provedor de HNV, você pode criar novas sub-redes lógicas (com os Pools de IP correspondente) que os hosts podem usar.  
  
  
## <a name="see-also"></a>Consulte também  
[Instalação e requisitos de preparação para implantar o controlador de rede](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[Rede definida pelo software &#40;SDN&#41;](../Software-Defined-Networking--SDN-.md)  
  


