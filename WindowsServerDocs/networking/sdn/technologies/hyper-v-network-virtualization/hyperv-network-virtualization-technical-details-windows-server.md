---
title: Detalhes técnicos da virtualização de rede Hyper-V no Windows Server 2016
description: Este tópico fornece informações técnicas sobre a virtualização de rede Hyper-V no Windows Server 2016
manager: grcusanz
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 94d45dd04708d58d880f3dcbc7a65e9586e02a29
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994408"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Detalhes técnicos da virtualização de rede Hyper-V no Windows Server 2016

>Aplica-se a: Windows Server 2016

A virtualização de servidores permite que várias instâncias do servidor sejam executadas simultaneamente em um único host físico, mas as instâncias do servidor são isoladas umas das outras. Cada máquina virtual opera essencialmente como se fosse o único servidor em execução no computador físico.

A virtualização de rede fornece um recurso semelhante, no qual várias redes virtuais (potencialmente com endereços IP sobrepostos) são executadas na mesma infraestrutura de rede física e cada rede virtual funciona como se fosse a única rede virtual em execução na infraestrutura de rede compartilhada. A figure 1 exibe essa relação.

![Virtualização do servidor versus virtualização de rede](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)

Figura 1: Virtualização do servidor x virtualização da rede

## <a name="hyper-v-network-virtualization-concepts"></a>Conceitos da Virtualização de Rede Hyper-V
Na HNV (virtualização de rede) do Hyper-V, um cliente ou locatário é definido como o "proprietário" de um conjunto de sub-redes IP que são implantadas em uma empresa ou Datacenter. Um cliente pode ser uma corporação ou empresa com vários departamentos ou unidades de negócios em um datacenter privado que exigem isolamento de rede ou um locatário em um data center público que é hospedado por um provedor de serviços. Cada cliente pode ter uma ou mais [redes virtuais](#VirtualNetworks) no datacenter, e cada rede virtual consiste em uma ou mais [sub-redes virtuais](#VirtualSubnets).

Há duas implementações HNV que estarão disponíveis no Windows Server 2016: HNVv1 e HNVv2.

-   **HNVv1**

    O HNVv1 é compatível com o Windows Server 2012 R2 e o System Center 2012 R2 Virtual Machine Manager (VMM). A configuração do HNVv1 depende do gerenciamento do WMI e dos cmdlets do Windows PowerShell (facilitado por meio do System Center VMM) para definir configurações de isolamento e mapeamentos de endereço de cliente (CA)-rede virtual para endereço físico (PA) e roteamento. Nenhum recurso adicional foi adicionado ao HNVv1 no Windows Server 2016 e nenhum novo recurso está planejado.

    *   DEFINIR o agrupamento e o HNV v1 não são compatíveis com a plataforma.

    o para usar gateways NVGRE de HA, os usuários precisam usar a equipe do LBFO ou nenhuma equipe. Ou

    o use gateways implantados do controlador de rede com o comutador conjunto agrupado.


-   **HNVv2**

    Um número significativo de novos recursos está incluído no HNVv2, que é implementado usando a extensão de encaminhamento da plataforma de filtragem virtual do Azure (VFP) no comutador do Hyper-V. O HNVv2 é totalmente integrado ao Microsoft Azure Stack que inclui o novo controlador de rede na pilha de SDN (rede definida pelo software).  A política de rede virtual é definida por meio do [controlador de rede](../../../sdn/technologies/network-controller/Network-Controller.md) da Microsoft usando uma API RESTful Northbound (NB) e encanada para um agente de host por meio de vários SouthBound INTEFACES (SBI), incluindo OVSDB. A política de programas do agente de host na extensão VFP do comutador do Hyper-V em que é imposta.

    > [!IMPORTANT]
    > Este tópico se concentra no HNVv2.

### <a name="virtual-network"></a><a name="VirtualNetworks"></a>Rede virtual

-   Cada rede virtual consiste em uma ou mais sub-redes virtuais. Uma rede virtual forma um limite de isolamento em que as máquinas virtuais em uma rede virtual só podem se comunicar entre si. Tradicionalmente, esse isolamento foi imposto usando VLANs com um intervalo de endereços IP segregado e uma marca 802.1 q ou ID de VLAN. Mas com HNV, o isolamento é imposto usando o encapsulamento NVGRE ou VXLAN para criar redes de sobreposição com a possibilidade de sobreposição de sub-redes IP entre clientes ou locatários.

-   Cada rede virtual tem uma RDID (ID de domínio de roteamento) exclusiva no host. Este RDID basicamente mapeia para uma ID de recurso para identificar o recurso REST de rede virtual no controlador de rede. O recurso REST da rede virtual é referenciado usando um namespace Uniform Resource Identifier (URI) com a ID de recurso acrescentada.

### <a name="virtual-subnets"></a><a name="VirtualSubnets"></a>Sub-redes virtuais

-   Uma sub-rede virtual implementa a semântica de sub-rede IP de Camada 3 das máquinas virtuais na mesma sub-rede virtual. A sub-rede virtual forma um domínio de difusão (semelhante a uma VLAN) e o isolamento é imposto usando o campo TNI (ID de rede de locatário NVGRE) ou VNI (identificador de rede VXLAN).

-   Cada sub-rede virtual pertence a uma única rede virtual (RDID) e recebe uma VSID (ID de sub-rede virtual) exclusiva usando a chave TNI ou VNI no cabeçalho de pacote encapsulado. O VSID deve ser exclusivo dentro do datacenter e está no intervalo de 4096 a 2 ^ 24-2.

Uma vantagem importante da rede virtual e do domínio de roteamento é que ele permite que os clientes Tragam suas próprias topologias de rede (por exemplo, sub-redes IP) para a nuvem. A Figura 2 mostra um exemplo onde a Contoso Corp tem duas redes separadas, Rede P&D e Rede de Vendas. Como essas redes têm diferentes IDs de domínio de roteamento, elas não podem interagir entre si. Ou seja, a contoso R&D net é isolada da Contoso Sales net, embora ambas sejam de propriedade da Contoso Corp. a contoso R&D net contém três sub-redes virtuais. Observe que a RDID e a VSID são exclusivas dentro de um datacenter.

![Redes de cliente e sub-redes virtuais](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)

Figura 2: Redes de cliente e sub-redes virtuais

**Encaminhamento da camada 2**

Na Figura 2, as máquinas virtuais no VSID 5001 podem ter seus pacotes encaminhados para máquinas virtuais que também estão no VSID 5001 por meio do comutador do Hyper-V. Os pacotes de entrada de uma máquina virtual no VSID 5001 são enviados para um VPort específico no comutador do Hyper-V. As regras de entrada (por exemplo, encap) e mapeamentos (por exemplo, cabeçalho de encapsulamento) são aplicadas pelo comutador do Hyper-V para esses pacotes. Os pacotes são encaminhados para um VPort diferente na opção do Hyper-V (se a máquina virtual de destino estiver anexada ao mesmo host) ou em um comutador Hyper-V diferente em um host diferente (se a máquina virtual de destino estiver localizada em um host diferente).

**Roteamento de camada 3**

Da mesma forma, as máquinas virtuais no VSID 5001 podem ter seus pacotes roteados para máquinas virtuais no VSID 5002 ou VSID 5003 pelo roteador distribuído HNV que está presente em cada VSwitch do host Hyper-V. Ao entregar o pacote para o comutador do Hyper-V, o HNV atualiza o VSID do pacote de entrada para o VSID da máquina virtual de destino. Isso ocorrerá somente se as duas VSIDs estiverem na mesma RDID.  Portanto, os adaptadores de rede virtual com RDID1 não podem enviar pacotes para adaptadores de rede virtual com RDID2 sem atravessar um gateway.

> [!NOTE]
> Na descrição do fluxo de pacotes acima, o termo "máquina virtual" realmente significa o adaptador de rede virtual na máquina virtual. O caso comum é quando uma máquina virtual tem apenas um único adaptador de rede virtual. Nesse caso, as palavras "máquina virtual" e "adaptador de rede virtual" podem significar a mesma coisa de forma conceitual.

Cada sub-rede virtual define uma sub-rede IP de Camada 3 e um limite de domínio de difusão de Camada 2 (L2) de maneira semelhante a uma VLAN. Quando uma máquina virtual transmite um pacote, o HNV usa a UR (replicação unicast) para fazer uma cópia do pacote original e substituir o IP de destino e o MAC pelos endereços de cada VM que estão no mesmo VSID.

> [!NOTE]
> Quando o Windows Server 2016 é fornecido, difusões e multicast de sub-rede são implementados usando a replicação unicast. Não há suporte para roteamento multicast entre sub-redes e IGMP.

Além de ser um domínio de difusão, a VSID fornece isolamento. Um adaptador de rede virtual no HNV é conectado a uma porta de comutador do Hyper-V que terá regras de ACL aplicadas diretamente à porta (recurso de virtualNetworkInterface REST) ou à sub-rede virtual (VSID) da qual ela faz parte.

A porta do comutador do Hyper-V deve ter uma regra ACL aplicada. Essa ACL pode ser permitir todos, negar tudo ou ser mais específica para permitir apenas certos tipos de tráfego com base em correspondência de 5 tuplas (IP de origem, IP de destino, porta de origem, porta de destino, protocolo) correspondente.

> [!NOTE]
> Extensões de comutador Hyper-V não funcionarão com HNVv2 na nova pilha de SDN (rede definida pelo software). O HNVv2 é implementado usando a extensão de comutador VFP (plataforma de filtragem virtual) do Azure que não pode ser usada em conjunto com qualquer outra extensão de comutador de terceiros.

## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Alternando e roteando na virtualização de rede Hyper-V
O HNVv2 implementa a alternância de camada 2 (L2) correta e a semântica de roteamento de camada 3 (L3) para funcionar assim como um comutador físico ou roteador funcionaria. Quando uma máquina virtual conectada a uma rede virtual HNV tenta estabelecer uma conexão com outra máquina virtual na mesma sub-rede virtual (VSID), primeiro será necessário aprender o endereço MAC da AC da máquina virtual remota. Se houver uma entrada ARP para o endereço IP da máquina virtual de destino na tabela ARP da máquina virtual de origem, o endereço MAC dessa entrada será usado. Se uma entrada não existir, a máquina virtual de origem enviará uma difusão ARP com uma solicitação para o endereço MAC correspondente ao endereço IP da máquina virtual de destino a ser retornado. A opção Hyper-V interceptará essa solicitação e a enviará ao agente do host. O agente do host procurará em seu banco de dados local um endereço MAC correspondente para o endereço IP da máquina virtual de destino solicitada.

> [!NOTE]
> O agente de host, agindo como o servidor OVSDB, usa uma variante do esquema VTEP para armazenar mapeamentos CA-PA, tabela MAC e assim por diante.

Se um endereço MAC estiver disponível, o agente de host injetará uma resposta ARP e a enviará de volta para a máquina virtual. Depois que a pilha de rede da máquina virtual tiver todas as informações de cabeçalho L2 necessárias, o quadro será enviado para a porta Hyper-V correspondente na opção V-. Internamente, o comutador do Hyper-V testa esse quadro em relação às regras de correspondência de N-tupla atribuídas à porta V e aplica determinadas transformações ao quadro com base nessas regras. O mais importante é que um conjunto de transformações de encapsulamento é aplicado para construir o cabeçalho de encapsulamento usando NVGRE ou VXLAN, dependendo da política definida no controlador de rede. Com base na política programada pelo agente do host, um mapeamento de CA-PA é usado para determinar o endereço IP do host Hyper-V onde reside a máquina virtual de destino. A opção Hyper-V garante que as regras de roteamento e as marcas de VLAN corretas sejam aplicadas ao pacote externo para que ele atinja o endereço PA remoto.

Se uma máquina virtual conectada a uma rede virtual HNV deseja criar uma conexão com uma máquina virtual em uma sub-rede virtual diferente (VSID), o pacote precisa ser roteado de acordo. O HNV pressupõe uma topologia em estrela em que há apenas um endereço IP no espaço da autoridade de certificação usado como o próximo salto para alcançar todos os prefixos de IP (ou seja, uma rota/gateway padrão). Atualmente, isso impõe uma limitação a uma única rota padrão e não há suporte para rotas não padrão.

### <a name="routing-between-virtual-subnets"></a>Roteamento Entre Sub-redes Virtuais
Em uma rede física, uma sub-rede IP é um domínio de camada 2 (L2) em que os computadores (virtuais e físicos) podem se comunicar diretamente entre si. O domínio L2 é um domínio de difusão em que as entradas ARP (IP: mapa de endereços MAC) são aprendidas por meio de solicitações ARP que são transmitidas em todas as interfaces e as respostas ARP são enviadas de volta para o host solicitante. O computador usa as informações de MAC aprendidas da resposta ARP para construir completamente o quadro L2, incluindo cabeçalhos Ethernet. No entanto, se um endereço IP estiver em uma sub-rede L3 diferente, a solicitação ARP não cruzará esse limite L3. Em vez disso, uma interface de roteador L3 (próximo salto ou gateway padrão) com um endereço IP na sub-rede de origem deve responder a essas solicitações ARP com seu próprio endereço MAC.

Na rede padrão do Windows, um administrador pode criar rotas estáticas e atribuí-las a uma interface de rede. Além disso, um "gateway padrão" geralmente é configurado para ser o endereço IP de próximo salto em uma interface em que os pacotes destinados à rota padrão (0.0.0.0/0) são enviados. Os pacotes serão enviados para esse gateway padrão se não existirem rotas específicas. Normalmente, esse é o roteador da rede física.  O HNV usa um roteador interno que faz parte de cada host e tem uma interface em cada VSID para criar um roteador distribuído para as redes virtuais.

Como o HNV pressupõe uma topologia em estrela, o roteador distribuído HNV atua como um gateway padrão único para todo o tráfego que está indo entre sub-redes virtuais que fazem parte da mesma rede VSID. O endereço usado como padrão do gateway padrão é o endereço IP mais baixo no VSID e é atribuído ao roteador distribuído HNV. Esse roteador distribuído permite uma maneira muito eficiente para todo o tráfego dentro de uma rede VSID ser roteado adequadamente porque cada host pode rotear diretamente o tráfego para o host apropriado sem precisar de um intermediário.  Isso é particularmente verdadeiro quando duas máquinas virtuais na mesma Rede VM, mas em diferentes Sub-redes Virtuais estão no mesmo host físico.  Como você verá mais adiante nesta seção, o pacote nunca terá que sair do host físico.

### <a name="routing-between-pa-subnets"></a>Roteamento entre sub-redes PA
Ao contrário do HNVv1 que alocou um endereço IP PA para cada sub-rede virtual (VSID), o HNVv2 agora usa um endereço IP PA por membro da equipe NIC de comutador incorporado (conjunto). A implantação padrão assume uma equipe de duas NICs e atribui dois endereços IP PA por host. Um único host tem IPs PA atribuídos da mesma sub-rede lógica do PA (provedor) na mesma VLAN. Duas VMs de locatário na mesma sub-rede virtual podem realmente estar localizadas em dois hosts diferentes que estão conectados a duas sub-redes lógicas de provedor diferentes. O HNV construirá os cabeçalhos IP externos para o pacote encapsulado com base no mapeamento CA-PA. No entanto, ele depende da pilha de TCP/IP do host para ARP para o gateway PA padrão e cria os cabeçalhos de Ethernet externos com base na resposta ARP. Normalmente, essa resposta ARP vem da interface SVI no comutador físico ou do roteador L3 onde o host está conectado. O HNV, portanto, depende do roteador L3 para rotear os pacotes encapsulados entre sub-redes lógicas/VLANs do provedor.

### <a name="routing-outside-a-virtual-network"></a>Roteamento Fora de uma Rede Virtual
A maioria das implantações de cliente exigirá a comunicação do ambiente de HNV com recursos que não fazem parte do ambiente de HNV. São necessários gateways de Virtualização de Rede para permitir a comunicação entre os dois ambientes. As infraestruturas que exigem um gateway HNV incluem nuvem privada e nuvem híbrida. Basicamente, os gateways de HNV são necessários para o roteamento de camada 3 entre redes internas e externas (incluindo NAT) ou entre diferentes sites e/ou nuvens (privadas ou públicas) que usam um túnel de VPN IPSec ou GRE.

Os gateways podem ser fornecidos em diferentes fatores forma físicos. Eles podem ser criados no Windows Server 2016, incorporados a um TOR (Top of rack) switch atuando como um gateway VXLAN, acessado por meio de um VIP (IP virtual) anunciado por um balanceador de carga, colocado em outros dispositivos de rede existentes ou pode ser um novo dispositivo de rede autônomo.

Para obter mais informações sobre as opções de gateway de RAS do Windows, consulte [Gateway de Ras](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).

## <a name="packet-encapsulation"></a>Encapsulamento de Pacote
Cada adaptador de rede virtual da HNV está associado a dois endereços IP:

-   **Endereço do cliente** (CA) o endereço IP atribuído pelo cliente, com base em sua infraestrutura de intranet. Esse endereço permite que o cliente troque o tráfego de rede com a máquina virtual como se ela não tivesse sido movida para uma nuvem pública ou privada. O CA é visível para a máquina virtual e está acessível para o cliente.

-   **Endereço do provedor** (PA) o endereço IP atribuído pelo provedor de hospedagem ou os administradores do datacenter com base em sua infraestrutura de rede física. O PA é exibido nos pacotes da rede que são trocados com o servidor Hyper-V que hospeda a máquina virtual. O PA está visível na rede física, mas não para a máquina virtual.

Os CAs mantêm a topologia de rede do cliente, que é virtualizada e separada dos endereços e da topologia de rede física real subjacente, conforme implementado pelos PAs. O diagrama a seguir mostra a relação conceitual entre os CAs de máquinas virtuais e os PAs da infraestrutura de rede resultantes da virtualização da rede.

![Diagrama conceitual da virtualização de rede na infraestrutura física](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)

Figura 6: Diagrama conceitual da virtualização de rede na infraestrutura física

No diagrama, as máquinas virtuais do cliente estão enviando pacotes de dados no espaço da autoridade de certificação, que atravessam a infraestrutura de rede física por meio de suas próprias redes virtuais ou "túneis". No exemplo acima, os túneis podem ser considerados como "envelopes" em volta dos pacotes de dados contoso e Fabrikam com os rótulos de envio verdes (endereços PA) a serem entregues do host de origem à esquerda para o host de destino à direita. A chave é como os hosts determinam os "endereços de envio" (PA) correspondentes à contoso e às autoridades de certificação da Fabrikam, como o "envelope" é colocado em volta dos pacotes e como os hosts de destino podem desencapsular os pacotes e entregar às máquinas virtuais de destino da Contoso e da Fabrikam corretamente.

Essa analogia simples destacou os principais aspectos da virtualização de rede:

-   Cada autoridade de certificação de máquina virtual é mapeada para um host físico PA. Pode haver diversos CAs associados ao mesmo PA.

-   As máquinas virtuais enviam pacotes de dados nos espaços da autoridade de certificação, que são colocados em um "envelope" com uma origem PA e um par de destino com base no mapeamento.

-   Os mapeamentos CA-PA devem permitir que os hosts diferenciem pacotes para máquinas virtuais do cliente diferentes.

Como resultado, o mecanismo de virtualização da rede consiste em virtualizar os endereços de rede usados pelas máquinas virtuais. O controlador de rede é responsável pelo mapeamento de endereço, e o agente de host mantém o banco de dados de mapeamento usando o esquema de MS_VTEP. A seção a seguir descreve o mecanismo real de virtualização de endereços.

## <a name="network-virtualization-through-address-virtualization"></a>Virtualização da rede por meio da virtualização de endereços
O HNV implementa sobreposição de redes de locatário usando o NVGRE (encapsulamento de roteamento genérico de virtualização de rede) ou a VXLAN (rede local extensível virtual).  VXLAN é o padrão.

### <a name="virtual-extensible-local-area-network-vxlan"></a>VXLAN (rede local extensível virtual)
O protocolo VXLAN (rede local extensível) ([RFC 7348](https://www.rfc-editor.org/info/rfc7348)) foi amplamente adotado no mercado, com suporte de fornecedores como Cisco, Brocade, Arista, Dell, HP e outros. O protocolo VXLAN usa UDP como o transporte. A porta de destino UDP atribuída pela IANA para VXLAN é 4789 e a porta de origem UDP deve ser um hash de informações do pacote interno a ser usado para a difusão de ECMP. Depois do cabeçalho UDP, um cabeçalho VXLAN é anexado ao pacote, que inclui um campo de 4 bytes reservado seguido por um campo de 3 bytes para o VNI (identificador de rede VXLAN)-VSID-seguido por outro campo de 1 byte reservado. Depois do cabeçalho VXLAN, o quadro L2 da CA original (sem o FCS de quadro Ethernet da CA) é acrescentado.

![Cabeçalho de pacote VXLAN](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)

### <a name="generic-routing-encapsulation-nvgre"></a>Encapsulamento de roteamento genérico (NVGRE)
Esse mecanismo de virtualização de rede usa o encapsulamento de roteamento genérico (NVGRE) como parte do cabeçalho de túnel. No NVGRE, o pacote da máquina virtual é encapsulado dentro de outro pacote. O cabeçalho desse novo pacote tem os endereços IP do PA de origem e de destino, além da ID da Sub-rede Virtual, que é armazenada no campo Key do cabeçalho do GRE, como mostrado na Figura 7.

![Encapsulamento NVGRE](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)

Figura 7: Virtualização de rede – encapsulamento NVGRE

A ID de sub-rede virtual permite que os hosts identifiquem a máquina virtual do cliente para qualquer pacote específico, mesmo que o PA e as autoridades de certificação nos pacotes possam se sobrepor. Isso permite que todas as máquinas virtuais no mesmo host compartilhem um único PA, como mostrado na Figura 7.

O compartilhamento do PA tem um grande impacto sobre a escalabilidade da rede. O número de endereços IP e MAC que devem ser aprendidos pela infraestrutura de rede pode ser reduzido significativamente. Por exemplo, se cada host final tiver uma média de 30 máquinas virtuais, o número de endereços IP e MAC que precisam ser aprendidos pela infraestrutura de rede é reduzido por um fator de 30. As IDs de Sub-rede Virtual inseridas nos pacotes também habilitam a fácil correlação de pacotes com os clientes reais.

O esquema de compartilhamento PA para o Windows Server 2012 R2 é um PA por VSID por host. Para o Windows Server 2016, o esquema é um PA por membro da equipe NIC.

Com o Windows Server 2016 e posterior, o HNV dá suporte total a NVGRE e VXLAN pronta para uso; Ele não requer atualização ou compra de novo hardware de rede, como NICs (adaptadores de rede), comutadores ou roteadores. Isso ocorre porque esses pacotes na transmissão são um pacote de IP regular no espaço PA, que é compatível com a infraestrutura de rede de hoje.  No entanto, para obter o melhor desempenho, use NICs com suporte com os drivers mais recentes que dão suporte a descarregamentos de tarefas.

## <a name="multi-tenant-deployment-example"></a>Exemplo de implantação multilocatário
O diagrama a seguir mostra um exemplo de implantação de dois clientes localizados em um datacenter na nuvem com a relação CA-PA definida pelas políticas de rede.

![Exemplo de implantação multilocatário](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)

Figura 8: Exemplo de implantação multilocatário

Considere o exemplo na Figura 8. Antes de migrar para o provedor de hospedagem do serviço IaaS compartilhado:

-   A Contoso Corp executava um SQL Server (chamado **SQL**) no endereço IP 10.1.1.11 e um servidor Web (chamado **Web**) no endereço IP 10.1.1.12, que usa seu SQL Server para transações de banco de dados.

-   A Fabrikam Corp executava um SQL Server, também chamado **SQL** e atribuiu o endereço IP 10.1.1.11 e um servidor Web, também chamado **Web**, e também no endereço IP 10.1.1.12, que usa seu SQL Server para transações de banco de dados.

Vamos pressupor que o provedor de serviço de hospedagem criou anteriormente a rede lógica de provedor (PA) por meio do controlador de rede para corresponder à topologia de rede física. O controlador de rede aloca dois endereços IP PA do prefixo IP da sub-rede lógica onde os hosts estão conectados. O controlador de rede também indica a marca de VLAN apropriada para aplicar os endereços IP.

Usando o controlador de rede, a Contoso Corp e a Fabrikam Corp criam sua rede virtual e sub-redes que são apoiadas pela rede lógica de provedor (PA) especificada pelo provedor de serviços de hospedagem. A Contoso Corp e Fabrikam Corp migram seus respectivos SQL Servers e servidores Web para o serviço IaaS compartilhado do mesmo provedor de hospedagem onde, coincidentemente, executam as máquinas virtuais **SQL** no Host Hyper-V 1 e as máquinas virtuais **Web** (IIS7) no Host Hyper-V 2. Todas as máquinas virtuais mantêm seus endereços IP de intranet originais (seus CAs).

As duas empresas recebem a seguinte VSID (ID de sub-rede virtual) pelo controlador de rede, conforme indicado abaixo.  O agente de host em cada um dos hosts do Hyper-V recebe os endereços IP PA alocados do controlador de rede e cria dois vNICs de host PA em um compartimento de rede não padrão. Uma interface de rede é atribuída a cada um desses vNICs de host onde o endereço IP PA é atribuído, conforme mostrado abaixo:

-   As máquinas virtuais da Contoso Corp VSID e PAs: **VSID** é 5001, o **SQL PA** é 192.168.1.10, o **Web PA** é 192.168.2.20

-   As máquinas virtuais VSID e PAs da Fabrikam Corp: **VSID** é 6001, o **SQL PA** é 192.168.1.10, o **Web PA** é 192.168.2.20

O controlador de rede hospeda todas as políticas de rede (incluindo o mapeamento de CA-PA) para o agente de host SDN, que manterá a política em um repositório persistente (em tabelas de banco de dados OVSDB).

Quando a máquina virtual da Contoso Corp (10.1.1.12) no host do Hyper-V 2 cria uma conexão TCP com a SQL Server em 10.1.1.11, acontece o seguinte:

-   ARPs VM para o endereço MAC de destino de 10.1.1.11

-   A extensão VFP no vSwitch intercepta esse pacote e o envia para o agente de host SDN

-   O agente de host SDN examina seu repositório de política para o endereço MAC para 10.1.1.11

-   Se um MAC for encontrado, o agente de host injetará uma resposta ARP de volta para a VM

-   Se um MAC não for encontrado, nenhuma resposta será enviada e a entrada ARP na VM para 10.1.1.11 será marcada como inacessível.

-   A VM agora constrói um pacote TCP com os cabeçalhos de IP e Ethernet de CA corretos e o envia para o vSwitch

-   A extensão de encaminhamento VFP no vSwitch processa esse pacote por meio das camadas VFP (descritas abaixo) atribuídas à porta de vSwitch de origem na qual o pacote foi recebido e cria uma nova entrada de fluxo na tabela de fluxo unificado VFP

-   O mecanismo VFP executa a correspondência de regras ou a pesquisa de tabela de fluxo para cada camada (por exemplo, camada de rede virtual) com base nos cabeçalhos IP e Ethernet.

-   A regra correspondente na camada de rede virtual faz referência a um espaço de mapeamento CA-PA e executa o encapsulamento.

-   O tipo de encapsulamento (VXLAN ou NVGRE) é especificado na camada de VNet junto com o VSID.

-   No caso do encapsulamento VXLAN, um cabeçalho UDP externo é construído com o VSID de 5001 no cabeçalho VXLAN.
    Um cabeçalho IP externo é construído com o endereço PA de origem e destino atribuído ao host Hyper-V 2 (192.168.2.20) e o host Hyper-V 1 (192.168.1.10), respectivamente, com base no armazenamento de política do agente de host SDN.

-   Em seguida, esse pacote flui para a camada de roteamento PA em VFP.

-   A camada de roteamento PA no VFP fará referência ao compartimento de rede usado para o tráfego de espaço PA e uma ID de VLAN e usará a pilha TCP/IP do host para encaminhar o pacote PA para o host Hyper-V 1 corretamente.

-   Após o recebimento do pacote encapsulado, o host do Hyper-V 1 recebe o pacote no compartimento de rede PA e o encaminha para o vSwitch.

-   O VFP processa o pacote por meio de suas camadas de VFP e cria uma nova entrada de fluxo na tabela de fluxo unificado VFP.

-   O mecanismo VFP corresponde às regras de entrada na camada de rede virtual e retira os cabeçalhos Ethernet, IP e VXLAN do pacote encapsulado externo.

-   Em seguida, o mecanismo VFP encaminha o pacote para a porta vSwitch à qual a VM de destino está conectada.

Um processo semelhante para o tráfego entre as máquinas virtuais **Web** e **SQL** da Fabrikam Corp usa as configurações de política de HNV para a Fabrikam Corp. Como resultado, com a HNV, as máquinas virtuais da Fabrikam Corp e da Contoso Corp interagem como se estivessem em suas intranets originais. Elas nunca podem interagir entre si, embora estejam usando os mesmos endereços IP.

Os endereços separados (CAs e PAs), as configurações de política dos hosts Hyper-V e a conversão de endereço entre a CA e o PA para o tráfego de máquina virtual de entrada e saída isolam esses conjuntos de servidores usando a chave NVGRE ou VLXAN VNID. Além disso, os mapeamentos de virtualização e a conversão separam a arquitetura da rede virtual da infraestrutura de rede da rede física. Embora os servidores **SQL** e **Web** da Contoso e **SQL** e **Web** da Fabrikam residam em suas próprias sub-redes IP do CA (10.1.1/24), sua implantação física ocorre em dois hosts em sub-redes PA diferentes, 192.168.1/24 e 192.168.2/24, respectivamente. A implicação é que o provisionamento de máquinas virtuais entre sub-redes e a migração ao vivo se tornam possíveis com a HNV.

## <a name="hyper-v-network-virtualization-architecture"></a>Arquitetura da Virtualização de Rede Hyper-V
No Windows Server 2016, o HNVv2 é implementado usando a plataforma de filtragem virtual do Azure (VFP), que é uma extensão de filtragem de NDIS dentro do comutador do Hyper-V. O principal conceito de VFP é o de um mecanismo de fluxo de ação de correspondência com uma API interna exposta ao agente de host SDN para programação de diretiva de rede. O agente de host SDN em si recebe a diretiva de rede do controlador de rede nos canais de comunicação OVSDB e WCF SouthBound. Não é apenas a política de rede virtual (por exemplo, mapeamento CA-PA) programada usando o VFP, mas políticas adicionais, como ACLs, QoS e assim por diante.

A hierarquia de objetos para o vSwitch e a extensão de encaminhamento VFP é a seguinte:

-   vSwitch

    -   Gerenciamento de NIC externa

    -   Descarregamentos de hardware NIC

    -   Regras de encaminhamento global

    -   Porta

        -   Camada de encaminhamento de saída para fixação de cabelo

        -   Listas de espaços para mapeamentos e pools de NAT

        -   Tabela de fluxo unificado

        -   Camada VFP

            -   Tabela de fluxo

            -   Agrupar

            -   Regra

                -   As regras podem referenciar espaços

No VFP, uma camada é criada por tipo de política (por exemplo, rede virtual) e é um conjunto genérico de tabelas de regras/fluxos. Ele não tem nenhuma funcionalidade intrínseca até que regras específicas sejam atribuídas a essa camada para implementar essa funcionalidade. Cada camada recebe uma prioridade e as camadas são atribuídas a uma porta por prioridade crescente. As regras são organizadas em grupos com base principalmente na direção e na família de endereços IP. Os grupos também recebem uma prioridade e, no máximo, uma regra de um grupo pode corresponder a um determinado fluxo.

A lógica de encaminhamento para o vSwitch com a extensão VFP é a seguinte:

-   Processamento de entrada (entrada do ponto de vista do pacote de entrada em uma porta)

-   Encaminhamento

-   Processamento de saída (saída do ponto de vista do pacote deixando uma porta)

O VFP dá suporte ao encaminhamento de MAC interno para os tipos de encapsulamento NVGRE e VXLAN, bem como para o encaminhamento baseado em VLAN de MAC externo.

A extensão VFP tem um caminho lento e um caminho rápido para percurso de pacote. O primeiro pacote em um fluxo deve atravessar todos os grupos de regras em cada camada e fazer uma pesquisa de regra que é uma operação cara. No entanto, quando um fluxo é registrado na tabela de fluxo unificado com uma lista de ações (com base nas regras correspondentes), todos os pacotes subsequentes serão processados com base nas entradas da tabela de fluxo unificado.

A política de HNV é programada pelo agente do host. Cada adaptador de rede de máquina virtual é configurado com um endereço IPv4. Esses são os CAs que serão usados pelas máquinas virtuais para se comunicarem entre si e eles são transportados nos pacotes IP das máquinas virtuais. HNV encapsula o quadro de CA em um quadro PA com base nas políticas de virtualização de rede armazenadas no banco de dados do agente do host.

![Arquitetura da HNV](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)

Figura 9: Arquitetura de HNV

## <a name="summary"></a>Resumo
Os datacenters baseados em nuvem podem oferecer diversos benefícios, como escalabilidade aprimorada e melhor utilização de recursos. Para aproveitar esses possíveis benefícios, é necessária uma tecnologia que basicamente trate dos problemas de escalabilidade multilocatário em um ambiente dinâmico. A HNV foi projetada para lidar com essas questões e também para aprimorar a eficiência operacional do datacenter, separando a topologia da rede virtual para a topologia da rede física. Com base em um padrão existente, o HNV é executado no datacenter atual e opera com a infraestrutura existente do VXLAN. Os clientes com HNV agora podem consolidar seus data centers em uma nuvem privada ou estender diretamente seus data centers para um ambiente de provedor de servidor de hospedagem com uma nuvem híbrida.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Consulte também
Para saber mais sobre o HNVv2, consulte os links a seguir:


|       Tipo de conteúdo       |                                                                                                                                              Referências                                                                                                                                              |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Recursos da comunidade**  |                                                                -   [Blog da arquitetura de nuvem privada](https://blogs.technet.com/b/privatecloud)<br />-Faça perguntas:[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)                                                                |
|         **RFC**          |                                                                   -   [RFC de rascunho do NVGRE](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN-RFC 7348](https://www.rfc-editor.org/info/rfc7348)                                                                    |
| **Tecnologias relacionadas** | -Para obter detalhes técnicos de virtualização de rede Hyper-V no Windows Server 2012 R2, consulte [detalhes técnicos da virtualização de rede do Hyper-v](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134174(v=ws.11))<br />-   [Controlador de rede](../../../sdn/technologies/network-controller/Network-Controller.md) |