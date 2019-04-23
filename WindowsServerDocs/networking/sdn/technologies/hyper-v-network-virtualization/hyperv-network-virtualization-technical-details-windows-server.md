---
title: Detalhes técnicos de virtualização de rede Hyper-V no Windows Server 2016
description: Este tópico fornece informações técnicas sobre virtualização de rede do Hyper-V no Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8db47479d0c6e6014a7df6cecd59b1e87920b76a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887547"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Detalhes técnicos de virtualização de rede Hyper-V no Windows Server 2016

>Aplica-se a:  Windows Server 2016

A virtualização de servidores permite que várias instâncias do servidor sejam executadas simultaneamente em um único host físico, mas as instâncias do servidor são isoladas umas das outras. Cada máquina virtual opera essencialmente como se fosse o único servidor em execução no computador físico.  
  
Virtualização de rede fornece uma funcionalidade semelhante, no quais várias redes virtuais (possivelmente com endereços IP sobrepostos) são executadas na mesma infraestrutura de rede física e cada rede virtual opera como se fosse a única rede virtual em execução em a infraestrutura de rede compartilhada. A figure 1 exibe essa relação.  
  
![Virtualização do servidor versus virtualização de rede](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  
  
Figura 1: Virtualização do servidor versus virtualização de rede  
  
## <a name="hyper-v-network-virtualization-concepts"></a>Conceitos da Virtualização de Rede Hyper-V  
Em virtualização de rede Hyper-V (HNV), um cliente ou Locatário é definido como "proprietário" de um conjunto de sub-redes IP que são implantados em um enterprise ou datacenter. Um cliente pode ser uma corporação ou empresa com vários departamentos ou unidades de negócios em um datacenter privado que requerem isolamento de rede ou um locatário em um datacenter público que é hospedado por um provedor de serviço. Cada cliente pode ter um ou mais [redes virtuais](#VirtualNetworks) no datacenter e cada virtual de rede consiste de um ou mais [subredes Virtual](#VirtualSubnets).  
  
Há duas implementações de HNV que estarão disponíveis no Windows Server 2016: HNVv1 e HNVv2.  
  
-   **HNVv1**  
  
    HNVv1 é compatível com o Windows Server 2012 R2 e System Center 2012 R2 Virtual Machine Manager (VMM). Configuração para HNVv1 baseia-se no gerenciamento de WMI e cmdlets do Windows PowerShell (facilitado por meio do System Center VMM) para definir as configurações de isolamento e cliente CA (endereço) – rede virtual - para mapeamentos de endereço físico (PA) e o roteamento. Não há recursos adicionais foram adicionados ao HNVv1 no Windows Server 2016 e não há novos recursos planejados.  
    
    • Defina agrupamento e HNV V1 não são compatíveis pela plataforma.
    
    o para usar usuários de gateways de alta disponibilidade NVGRE precisa para qualquer um use equipe LBFO ou nenhuma equipe. Ou
    
    gateways de s implantado de controlador de rede de uso com o conjunto agrupado de comutador.

  
-   **HNVv2**  
  
    Um número significativo de novos recursos está incluído no HNVv2 que é implementado usando o Azure Virtual filtragem VFP (plataforma) a extensão de comutador do Hyper-V de encaminhamento. HNVv2 é totalmente integrado com o Microsoft Azure Stack, que inclui o novo controlador de rede na pilha de Software Defined Networking (SDN).  Política de rede virtual é definida por meio do Microsoft [controlador de rede](../../../sdn/technologies/network-controller/Network-Controller.md) usando um RESTful NorthBound (NB) API e inseridas para um agente de Host por meio de vários SouthBound Intefaces (SBI) incluindo OVSDB. A política de programas de agente de Host na extensão VFP do comutador do Hyper-V em que ela é imposta.  
  
    > [!IMPORTANT]  
    > Este tópico se concentra em HNVv2.  
  
### <a name="VirtualNetworks"></a>Rede virtual  
  
-   Cada rede virtual consiste em uma ou mais sub-redes virtuais. Uma rede virtual forma um limite de isolamento onde as máquinas virtuais em uma rede virtual podem se comunicar somente com o outro. Tradicionalmente, esse isolamento foi imposto usando VLANs com um intervalo de endereços IP segregado e 802.1q marca ou ID de VLAN. Mas, com HNV, o isolamento é imposto usando encapsulamento NVGRE ou VXLAN para criar redes de sobreposição com a possibilidade de sobreposição de sub-redes IP entre os clientes ou locatários.  
  
-   Cada rede virtual tem um único domínio RDID (ID roteamento) no host. Este RDID aproximadamente mapeia para uma ID de recurso para identificar a rede virtual do recurso REST no controlador de rede. A recurso REST de rede virtual é referenciada usando um namespace de identificador de recurso uniforme (URI) com a ID de recurso acrescentados.  
  
### <a name="VirtualSubnets"></a>Subredes virtuais  
  
-   Uma sub-rede virtual implementa a semântica de sub-rede IP de Camada 3 das máquinas virtuais na mesma sub-rede virtual. A sub-rede virtual forma um domínio de difusão (semelhante a uma VLAN) e o isolamento é imposto por meio do campo de ID de rede de locatário de NVGRE (TNI) ou o identificador de rede VXLAN (VNI).  
  
-   Cada sub-rede virtual pertence a uma única rede virtual (RDID) e ele é atribuído um exclusivo sub-rede Virtual ID (VSID) usando a chave TNI ou VNI no cabeçalho do pacote encapsulado. A VSID deve ser exclusiva dentro do datacenter e está no intervalo entre 4096 e 2 ^ 24-2.  
  
Uma vantagem importante da rede virtual e do domínio de roteamento é que ele permite que os clientes tragam suas próprias topologias de rede (por exemplo, sub-redes IP) para a nuvem. A Figura 2 mostra um exemplo onde a Contoso Corp tem duas redes separadas, Rede P&D e Rede de Vendas. Como essas redes têm diferentes IDs de domínio de roteamento, elas não podem interagir entre si. Ou seja, a Rede R&D da Contoso é isolada da Rede de Vendas da Contoso mesmo ambas sendo de propriedade da Contoso Corp. A Rede R&D da Contoso contém três sub-redes virtuais. Observe que a RDID e a VSID são exclusivas dentro de um datacenter.  
  
![Redes de cliente e sub-redes virtuais](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  
  
Figura 2: Redes de cliente e sub-redes virtuais  
  
**Encaminhamento de camada 2**  
  
Na Figura 2, as máquinas virtuais em VSID 5001 podem ter seus pacotes encaminhados para máquinas virtuais que estão também em VSID 5001 através do comutador do Hyper-V. Os pacotes de entrada de uma máquina virtual em VSID 5001 são enviados para um VPort específico no comutador do Hyper-V. Regras de entrada (por exemplo, encap) e mapeamentos (por exemplo, o cabeçalho de encapsulamento) são aplicados pelo comutador Hyper-V para esses pacotes. Os pacotes são então encaminhados para um VPort diferente no comutador do Hyper-V (se a máquina virtual de destino está conectada no mesmo host) ou a um comutador diferente do Hyper-V em um host diferente (se a máquina virtual de destino está localizada em um host diferente).  
  
**Roteamento na camada 3**  
  
Da mesma forma, as máquinas virtuais em VSID 5001 podem ter seus pacotes roteados para máquinas virtuais em VSID 5002 ou VSID 5003 pelo roteador distribuído HNV que está presente em VSwitch do cada host Hyper-V. Após a entregar o pacote para o Hyper-V switch, o HNV atualiza o VSID do pacote de entrada para a VSID da máquina virtual de destino. Isso ocorrerá somente se as duas VSIDs estiverem na mesma RDID.  Portanto, os adaptadores de rede virtual com RDID1 não é possível enviar pacotes para adaptadores de rede virtual com RDID2 sem percorrer um gateway.  
  
> [!NOTE]  
> Na descrição do fluxo de pacotes acima, o termo "máquina virtual", na verdade, significa que o adaptador de rede virtual na máquina virtual. O caso comum é quando uma máquina virtual tem apenas um único adaptador de rede virtual. Nesse caso, as palavras "máquina virtual" e "adaptador de rede virtual" podem significar, conceitualmente a mesma coisa.  
  
Cada sub-rede virtual define uma sub-rede IP de Camada 3 e um limite de domínio de difusão de Camada 2 (L2) de maneira semelhante a uma VLAN. Quando uma máquina virtual difunde um pacote, a HNV utiliza a replicação de Unicast (UR) para fazer uma cópia do pacote original e substitua o MAC e IP de destino com os endereços de cada VM que estão na mesma VSID.  
  
> [!NOTE]  
> Quando o Windows Server 2016 for lançado, multicasts de difusão e de sub-rede serão implementados usando a replicação de unicast. Não há suporte para o roteamento de multicast de sub-rede cruzado e IGMP.  
  
Além de ser um domínio de difusão, a VSID fornece isolamento. Um adaptador de rede virtual na HNV é conectado à porta de comutador do Hyper-V que terá regras de ACL aplicadas diretamente para a porta (recurso do REST virtualNetworkInterface) ou para a sub-rede virtual (VSID) do qual ele é uma parte.  
  
A porta do comutador do Hyper-V deve ter uma regra ACL aplicada. Essa ACL pode ser permitir todos, negar tudo, ou ser mais específico para permitir que somente determinados tipos de tráfego com base na correspondência de 5 tuplas (IP de origem, IP de destino, porta de origem, porta de destino, protocolo).  
  
> [!NOTE]  
> Extensões de comutador do Hyper-V não funcionarão com HNVv2 na nova pilha de Software Defined Networking (SDN). HNVv2 é implementado usando a extensão do comutador Azure Virtual filtragem VFP (plataforma) que não pode ser usada em conjunto com qualquer outra extensão de comutador 3ª parte.  
  
## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Comutação e roteamento na virtualização de rede Hyper-V  
HNVv2 implementa a alternância correto de camada 2 (L2) e a semântica de roteamento de camada 3 (L3) para funcionar apenas como um comutador físico ou roteador funcionaria. Quando uma máquina virtual conectada a uma rede virtual da HNV tenta estabelecer uma conexão com outra máquina virtual na mesma sub-rede virtual (VSID) primeiro será necessário saber o endereço MAC de autoridade de certificação da máquina virtual remota. Se houver uma entrada de ARP para o endereço IP da máquina virtual de destino na tabela de ARP da máquina virtual de origem, o endereço MAC dessa entrada é usado. Se uma entrada não existir, a máquina virtual de origem enviará que um ARP transmissão com uma solicitação para o endereço MAC correspondente ao endereço IP da máquina virtual de destino a ser retornado. O comutador do Hyper-V será interceptar essa solicitação e enviá-lo para o agente de Host. O agente de Host aparecerá no seu banco de dados local para um endereço MAC correspondente para o endereço IP da máquina virtual de destino solicitado.  
  
> [!NOTE]  
> O agente de Host, atuando como servidor OVSDB, usa uma variante do esquema VTEP para armazenar os mapeamentos CA-PA, tabela de MAC e assim por diante.  
  
Se um endereço MAC está disponível, o agente de Host injeta uma resposta ARP e envia novamente para a máquina virtual. Depois de pilha da rede da máquina virtual tem todas as L2 cabeçalho informações necessárias, o quadro é enviado para a porta correspondente do Hyper-V em que a opção-V. Internamente, o comutador do Hyper-V testa esse quadro com relação a regras de correspondência de tupla de N atribuído à porta V e se aplica a determinadas transformações para o quadro com base nessas regras. Mais importante, um conjunto de transformações de encapsulamento é aplicado para construir o cabeçalho de encapsulamento usando NVGRE ou VXLAN, dependendo da política definida no controlador de rede. Com base na política programada pelo agente de Host, um mapeamento de CA-PA é usado para determinar o endereço IP do host do Hyper-V em que a máquina virtual de destino reside. O comutador do Hyper-V garante que as regras de roteamento corretas e marcas de VLAN são aplicadas ao pacote externo para que ele alcance o endereço de PA remoto.  
  
Se uma máquina virtual conectada a uma rede virtual da HNV quer criar uma conexão com uma máquina virtual em uma sub-rede diferente virtual (VSID), o pacote precisa ser roteadas adequadamente. A HNV pressupõe uma topologia em estrela em que há apenas um endereço IP no espaço do CA usado como o próximo salto para atingir todos os prefixos IP (que significa uma rota/gateway padrão). Atualmente, isso impõe um limite para uma única rota padrão e não há suporte para as rotas não padrão.  
  
### <a name="routing-between-virtual-subnets"></a>Roteamento Entre Sub-redes Virtuais  
Em uma rede física, uma sub-rede IP é um domínio de camada 2 (L2) em que computadores (virtuais e físicos) podem se comunicar diretamente entre si. O domínio de L2 é um domínio de difusão em que entradas de ARP (mapa de endereço IP:MAC) são aprendidas por meio de solicitações de ARP que são transmitidas em todas as interfaces e respostas de ARP são enviadas para o host do solicitante. O computador usa as informações de MAC aprendidas com a resposta ARP para construir completamente o quadro de L2, incluindo os cabeçalhos de Ethernet. No entanto, se for um endereço IP em uma sub-rede diferente de L3, a solicitação ARP não ultrapassar o limite esse L3. Em vez disso, uma interface de roteador (gateway padrão ou de próximo salto) L3 com um endereço IP na sub-rede de origem deve responder a essas solicitações de ARP com seu próprio endereço MAC.  
  
No sistema de rede padrão do Windows, um administrador pode criar rotas estáticas e atribuí-los para um adaptador de rede. Além disso, um "gateway padrão" geralmente é configurado para ser o endereço IP do próximo salto em uma interface em que os pacotes destinados para a rota padrão (0.0.0.0/0) são enviados. Pacotes são enviados para este gateway padrão se nenhuma rotas específicas existe. Normalmente, esse é o roteador da rede física.  A HNV utiliza um roteador interno que é parte de cada host e tem uma interface em cada VSID para criar um roteador distribuído para as redes virtuais.  
  
Uma vez que HNV pressupõe uma topologia em estrela, o roteador distribuído de HNV atua como um único gateway padrão para todo o tráfego entre sub-redes virtuais que fazem parte da mesma rede VSID. O endereço usado como os padrões de gateway padrão para o endereço IP mais baixo na VSID e é atribuído ao roteador distribuído da HNV. Esse roteador distribuído permite uma maneira muito eficiente para todo o tráfego dentro de uma rede VSID sejam roteadas adequadamente porque cada host pode rotear o tráfego diretamente para o host apropriado sem a necessidade de um intermediário.  Isso é particularmente verdadeiro quando duas máquinas virtuais na mesma Rede VM, mas em diferentes Sub-redes Virtuais estão no mesmo host físico.  Como você verá mais adiante nesta seção, o pacote nunca terá que sair do host físico.  
  
### <a name="routing-between-pa-subnets"></a>O roteamento entre sub-redes PA  
Em contraste com HNVv1 que alocados para cada sub-rede Virtual (VSID) de um endereço IP do PA, HNVv2 agora usa um endereço IP do PA por membro da equipe NIC Switch-Embedded Teaming (SET). A implantação padrão pressupõe que uma equipe de dois NICs e atribui dois endereços IP do PA por host. Um único host tem IPs PA atribuída da mesma sub-rede lógica do provedor (PA) na mesma VLAN. Duas VMs do locatário na mesma sub-rede virtual, de fato, podem estar localizado em dois hosts diferentes que estão conectados à duas sub-redes lógicas de provedor diferente. A HNV construirão os cabeçalhos IP externos para o pacote encapsulado com base no mapeamento de CA-PA. No entanto, ele se baseia na pilha de TCP/IP de host ao ARP para o gateway de PA padrão e, em seguida, cria os cabeçalhos de Ethernet externos com base na resposta ARP. Normalmente, essa resposta ARP vem da interface SVI no L3 roteador ou comutador físico em que o host está conectado. A HNV, portanto, se baseia no roteador L3 para rotear os pacotes encapsulados entre sub-redes lógicas do provedor / VLANs.  
  
### <a name="routing-outside-a-virtual-network"></a>Roteamento Fora de uma Rede Virtual  
A maioria das implantações de cliente exigirá a comunicação do ambiente de HNV com recursos que não fazem parte do ambiente de HNV. São necessários gateways de Virtualização de Rede para permitir a comunicação entre os dois ambientes. Infraestruturas exigindo um Gateway de HNV incluem a nuvem privada e nuvem híbrida. Basicamente, os gateways de HNV são necessários para roteamento de camada 3 entre redes (físicos) internos e externos (incluindo NAT) ou entre diferentes sites e/ou nuvens (privadas ou públicas) que usam um túnel VPN IPSec ou GRE.  
  
Os gateways podem ser fornecidos em diferentes fatores forma físicos. Eles podem ser criados no Windows Server 2016, incorporado a um comutador de topo de Rack (TOR) atuando como um Gateway VXLAN, acessado por meio de um IP Virtual (VIP) anunciado por um balanceador de carga, colocado em outros dispositivos de rede existente ou pode ser um novo dispositivo de rede autônomo .  
  
Para obter mais informações sobre as opções de Gateway de RAS do Windows, consulte [Gateway de RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="packet-encapsulation"></a>Encapsulamento de Pacote  
Cada adaptador de rede virtual da HNV está associado a dois endereços IP:  
  
-   **Endereço do cliente** (CA) o endereço IP atribuído pelo cliente, com base em infraestrutura de sua intranet. Esse endereço permite que o cliente troque tráfego de rede com a máquina virtual como se ela não tivesse migrada para uma nuvem pública ou privada. O CA é visível para a máquina virtual e está acessível para o cliente.  
  
-   **Endereço do provedor** (PA) o endereço IP atribuído pelo provedor de hospedagem ou os administradores do datacenter com base em sua infraestrutura de rede física. O PA é exibido nos pacotes da rede que são trocados com o servidor Hyper-V que hospeda a máquina virtual. O PA está visível na rede física, mas não para a máquina virtual.  
  
Os CAs mantêm a topologia de rede do cliente, que é virtualizada e separada dos endereços e da topologia de rede física real subjacente, conforme implementado pelos PAs. O diagrama a seguir mostra a relação conceitual entre os CAs de máquinas virtuais e os PAs da infraestrutura de rede resultantes da virtualização da rede.  
  
![Diagrama conceitual da virtualização de rede na infraestrutura física](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  
  
Figura 6: Diagrama conceitual da virtualização de rede na infraestrutura física  
  
No diagrama, as máquinas virtuais do cliente está enviando pacotes de dados no espaço do CA, que ultrapassa a infraestrutura de rede física por meio de suas próprias redes virtuais, ou "túneis". No exemplo acima, é possível pensar nos túneis como "envelopes" ao redor dos pacotes de dados Contoso e Fabrikam com rótulos envio verdes (endereços PA) a serem entregues do host de origem à esquerda para o host de destino à direita. A chave é como os hosts determinam os "endereços de entrega" (PAS) correspondentes para a Contoso e Fabrikam da AC, como o "envelope" é colocado ao redor dos pacotes e como os hosts de destino podem descodificar os pacotes e entregar para a Contoso e Fabrikam destino virtual máquinas corretamente.  
  
Essa analogia simples destacou os principais aspectos da virtualização de rede:  
  
-   Cada autoridade de certificação de máquina virtual é mapeada para um host físico PA. Pode haver diversos CAs associados ao mesmo PA.  
  
-   As máquinas virtuais enviam pacotes de dados nos espaços do CA, que são colocados em um "envelope" com um par de origem e destino de PA com base no mapeamento.  
  
-   Os mapeamentos CA-PA devem permitir que os hosts diferenciem pacotes para máquinas virtuais do cliente diferentes.  
  
Como resultado, o mecanismo de virtualização da rede consiste em virtualizar os endereços de rede usados pelas máquinas virtuais. O controlador de rede é responsável pelo mapeamento do endereço e o agente de host mantém o banco de dados de mapeamento usando o esquema MS_VTEP. A seção a seguir descreve o mecanismo real de virtualização de endereços.  
  
## <a name="network-virtualization-through-address-virtualization"></a>Virtualização da rede por meio da virtualização de endereços  
A HNV implementa sobreposição de redes de locatário usando a rede virtualização genérico roteamento NVGRE (encapsulamento) ou o Virtual extensível Local VXLAN (rede).  VXLAN é o padrão.  
  
### <a name="virtual-extensible-local-area-network-vxlan"></a>Rede de área Local virtual extensível (VXLAN)  
O Virtual extensível Local VXLAN (rede) ([7348 RFC](https://www.rfc-editor.org/info/rfc7348)) protocolo foi amplamente adotado no mercado, com suporte dos fornecedores como Cisco, Brocade, Arista, Dell, HP e outros. O protocolo VXLAN usa UDP como o transporte. A porta de destino UDP atribuído pela IANA para VXLAN é 4789 e a porta UDP de origem deve ser um hash de informações do que o pacote interno a ser usado para ECMP difusão. Após o cabeçalho UDP, um cabeçalho VXLAN é acrescentado ao pacote que inclui um campo reservado de 4 bytes seguido por um campo de 3 bytes para a VXLAN rede identificador (VNI) - VSID - seguido de outro campo reservado de 1 byte. Após o cabeçalho VXLAN, o quadro de autoridade de certificação L2 original (sem quadro Ethernet de autoridade de certificação FCS) é acrescentado.  
  
![Cabeçalho do pacote VXLAN](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  
  
### <a name="generic-routing-encapsulation-nvgre"></a>Encapsulamento de roteamento genérico (NVGRE)  
Este mecanismo de virtualização de rede usa o genérico roteamento NVGRE (encapsulamento) como parte do cabeçalho do túnel. No NVGRE, o pacote da máquina virtual é encapsulado dentro de outro pacote. O cabeçalho desse novo pacote tem os endereços IP do PA de origem e de destino, além da ID da Sub-rede Virtual, que é armazenada no campo Key do cabeçalho do GRE, como mostrado na Figura 7.  
  
![Encapsulamento NVGRE](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  
  
Figura 7: Virtualização de rede – encapsulamento NVGRE  
  
A ID de subrede Virtual permite que os hosts identifiquem a máquina virtual de cliente para qualquer pacote específico, embora os PAS e a autoridade de certificação nos pacotes possam se sobrepor. Isso permite que todas as máquinas virtuais no mesmo host compartilhem um único PA, como mostrado na Figura 7.  
  
O compartilhamento do PA tem um grande impacto sobre a escalabilidade da rede. O número de endereços IP e MAC que devem ser aprendidos pela infraestrutura de rede pode ser reduzido significativamente. Por exemplo, se cada host final tiver uma média de 30 máquinas virtuais, o número de endereços IP e MAC que precisam ser aprendidos pela infraestrutura de rede é reduzido por um fator de 30. As IDs de Sub-rede Virtual inseridas nos pacotes também habilitam a fácil correlação de pacotes com os clientes reais.  
  
O PA de compartilhamento de esquema para o Windows Server 2012 R2 é um PA por VSID por host. Para o Windows Server 2016, o esquema é um PA por membro da equipe NIC.  
  
Com o Windows Server 2016 e posterior, a HNV totalmente compatível com NVGRE e VXLAN fora da caixa; ele não requer atualizar ou adquirir novo hardware de rede, como NICs (adaptadores de rede), comutadores ou roteadores. Isso ocorre porque esses pacotes durante a transmissão são pacote IP normal no espaço do PA, que é compatível com a infraestrutura de rede atual.  No entanto obter o melhor desempenho, use suporte NICs com os drivers mais recentes que dão suporte ao descarregamento de tarefa.  
  
## <a name="multi-tenant-deployment-example"></a>Exemplo de implantação multilocatário  
O diagrama a seguir mostra um exemplo de implantação de dois clientes localizados em um datacenter de nuvem com a relação CA-PA definida pelas políticas de rede.  
  
![Exemplo de implantação multilocatário](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  
  
Figura 8: Exemplo de implantação multilocatário  
  
Considere o exemplo na Figura 8. Antes de migrar para o provedor de hospedagem do serviço IaaS compartilhado:  
  
-   A Contoso Corp executava um SQL Server (chamado **SQL**) no endereço IP 10.1.1.11 e um servidor Web (chamado **Web**) no endereço IP 10.1.1.12, que usa seu SQL Server para transações de banco de dados.  
  
-   A Fabrikam Corp executava um SQL Server, também chamado **SQL** e atribuiu o endereço IP 10.1.1.11 e um servidor Web, também chamado **Web** , e também no endereço IP 10.1.1.12, que usa seu SQL Server para transações de banco de dados.  
  
Vamos pressupor que o provedor de serviços de hospedagem anteriormente criou a rede lógica do provedor (PA) por meio do controlador de rede para corresponder à sua topologia de rede física. O controlador de rede aloca dois endereços IP do PA de prefixo de IP da sub-rede lógica em que os hosts estão conectados. O controlador de rede também indica a marca VLAN apropriada para aplicar os endereços IP.  
  
Usando o controlador de rede, a Contoso Corp e Fabrikam Corp, em seguida, crie sua rede virtual e sub-redes que contam com a rede lógica do provedor (PA) especificado pelo provedor de serviço de hospedagem. A Contoso Corp e Fabrikam Corp migram seus respectivos SQL Servers e servidores Web para o serviço IaaS compartilhado do mesmo provedor de hospedagem onde, coincidentemente, executam as máquinas virtuais **SQL** no Host Hyper-V 1 e as máquinas virtuais **Web** (IIS7) no Host Hyper-V 2. Todas as máquinas virtuais mantêm seus endereços IP de intranet originais (seus CAs).  
  
Ambas as empresas são atribuídas a seguir Virtual sub-rede VSIDS (IDs) pelo controlador de rede, conforme indicado abaixo.  O agente de Host em cada um dos hosts do Hyper-V recebe os endereços IP do PA alocados do controlador de rede e cria duas vNICs do host PA em um compartimento de rede não-padrão. Uma interface de rede é atribuída a cada uma dessas vNICs do host em que o endereço IP do PA é atribuído como mostrado abaixo:  
  
-   Máquinas de virtuais da Contoso Corp VSID e PAs: **VSID** é 5001, **SQL PA** é 192.168.1.10, **PA Web** é 192.168.2.20  
  
-   Máquinas de virtuais da Fabrikam Corp VSID e PAs: **VSID** é 6001, **SQL PA** é 192.168.1.10, **PA Web** é 192.168.2.20  
  
O controlador de rede conecta a todas as políticas de rede (incluindo o mapeamento de CA-PA) para o agente de Host do SDN que manterá a política em um armazenamento persistente (em tabelas de banco de dados OVSDB).  
  
Quando a máquina virtual Web da Contoso Corp (10.1.1.12) no Host Hyper-V 2 cria uma conexão TCP para o SQL Server em 10.1.1.11, ocorre o seguinte:  
  
-   VM ARPs o endereço MAC de destino do 10.1.1.11  
  
-   A extensão VFP em vSwitch intercepta esse pacote e o envia para o agente de Host de SDN  
  
-   O agente de Host do SDN procura no seu repositório de políticas para o endereço MAC para 10.1.1.11  
  
-   Se for encontrado um MAC, o agente de Host injeta uma resposta ARP para a VM  
  
-   Se não for encontrado um MAC, nenhuma resposta é enviada e a entrada ARP na VM para 10.1.1.11 é marcado como inacessível.  
  
-   A VM agora constrói um pacote TCP com os cabeçalhos IP e Ethernet de autoridade de certificação corretos e o envia para o vSwitch  
  
-   O VFP encaminhamento de extensão de vSwitch processa esse pacote pelas camadas VFP (descritos abaixo) atribuído à porta vSwitch fonte no qual o pacote foi recebido e cria uma nova entrada de fluxo na tabela de fluxo unificada de VFP  
  
-   O mecanismo VFP executa a pesquisa de regra de correspondência ou tabela de fluxo para cada camada (por exemplo, a camada de rede virtual) com base em cabeçalhos de IP e Ethernet.  
  
-   A regra correspondente na camada de rede virtual faz referência a um espaço de mapeamento de CA-PA e executa o encapsulamento.  
  
-   O tipo de encapsulamento (VXLAN ou NVGRE) é especificado na camada de rede virtual, juntamente com a VSID.  
  
-   No caso do encapsulamento VXLAN, um cabeçalho UDP externo é construído com a VSID da 5001 no cabeçalho VXLAN.  
    Um cabeçalho IP externo é construído com o endereço de PA de origem e de destino atribuído ao Host do Hyper-V 2 (192.168.2.20) e do Hyper-V Host 1 (192.168.1.10), respectivamente, com base no repositório de políticas do agente de Host de SDN.  
  
-   Esse pacote, em seguida, fluem para a camada de roteamento de PA em VFP.  
  
-   A camada de roteamento de PA em VFP irá referenciar o compartimento de rede usado para o tráfego de espaço de PA e uma ID de VLAN e usar a pilha de TCP/IP do host para encaminhar o pacote de PA para o Host Hyper-V 1 corretamente.  
  
-   Após o recebimento do pacote encapsulado, o Host Hyper-V 1 recebe o pacote no compartimento de rede de PA e encaminhá-lo para o vSwitch.  
  
-   O VFP processa o pacote por meio de suas camadas VFP e crie uma nova entrada de fluxo na tabela de fluxo unificada VFP.  
  
-   O mecanismo VFP coincida com as regras de ingres na camada de rede virtual e ignora os cabeçalhos de Ethernet, o IP e a VXLAN do pacote encapsulado externa.  
  
-   O mecanismo VFP, em seguida, encaminha o pacote para a porta vSwitch ao qual a VM de destino está conectado.  
  
Um processo semelhante para o tráfego entre máquinas virtuais **SQL** e **Web** da Fabrikam Corp usa as configurações de política HNV para a Fabrikam Corp. Como resultado, com HNV, as máquinas virtuais da Fabrikan Corp e da Contoso Corp interagem como se estivesse em suas intranets originais. Elas nunca podem interagir entre si, embora estejam usando os mesmos endereços IP.  
  
Os endereços separados (CAs e PAs), as configurações de política dos hosts do Hyper-V e a conversão de endereço entre a autoridade de certificação e o PA para tráfego de entrada e saída máquinas de virtuais isolam esses conjuntos de servidores usando a chave de NVGRE ou o VNID VLXAN. Além disso, os mapeamentos de virtualização e a conversão separam a arquitetura da rede virtual da infraestrutura de rede da rede física. Embora os servidores **SQL** e **Web** da Contoso e **SQL** e **Web** da Fabrikam residam em suas próprias sub-redes IP do CA (10.1.1/24), sua implantação física ocorre em dois hosts em sub-redes PA diferentes, 192.168.1/24 e 192.168.2/24, respectivamente. A implicação é que o provisionamento de máquinas virtuais entre sub-redes e a migração ao vivo se tornam possíveis com a HNV.  
  
## <a name="hyper-v-network-virtualization-architecture"></a>Arquitetura da Virtualização de Rede Hyper-V  
No Windows Server 2016, HNVv2 é implementado usando o Azure Virtual filtragem VFP (plataforma) que é uma extensão de filtragem do NDIS dentro do comutador do Hyper-V. O conceito-chave do VFP é de um mecanismo de fluxo de ação de correspondência com uma API interna exposto para o agente de Host para a programação de diretiva de rede SDN. O próprio agente de Host de SDN recebe a política de rede do controlador de rede sobre os canais de comunicação OVSDB e SouthBound do WCF. Não é apenas a diretiva de rede virtual (por exemplo, o mapeamento de CA-PA) programado usando VFP mas políticas adicionais, como ACLs, QoS e assim por diante.  
  
A hierarquia de objeto para a extensão de encaminhamento de VFP e vSwitch é o seguinte:  
  
-   vSwitch  
  
    -   Gerenciamento de NIC externa  
  
    -   Descarregamentos de Hardware NIC  
  
    -   Regras de encaminhamento globais  
  
    -   Porta  
  
        -   Saída de encaminhamento de camada para fixação de cabelo  
  
        -   Lista de espaço para mapeamentos e pools de NAT  
  
        -   Tabela de fluxo unificada  
  
        -   Camada VFP  
  
            -   Tabela de fluxo  
  
            -   Grupo  
  
            -   Regra  
  
                -   As regras podem fazer referência a espaços  
  
No VFP, uma camada é criada por tipo de política (por exemplo, rede Virtual) e é um conjunto genérico de tabelas/fluxo de regra. Ele não tem nenhuma funcionalidade intrínseco até que as regras específicas sejam atribuídas para a camada para implementar essa funcionalidade. Cada camada é atribuída uma prioridade e camadas são atribuídas a uma porta por em ordem crescente de prioridade. As regras são organizadas em grupos com base principalmente na direção e a família de endereços IP. Grupos também recebem uma prioridade e no máximo, uma regra de um grupo pode corresponder a um determinado fluxo.  
  
A lógica de encaminhamento para o vSwitch com extensão VFP é da seguinte maneira:  
  
-   Processamento de entrada (do ponto de vista de pacote em uma porta de entrada)  
  
-   Encaminhamento  
  
-   Processamento de saída (egresso do ponto de vista de deixar uma porta de pacote)  
  
O VFP dá suporte ao encaminhamento de MAC interno para os tipos de encapsulamento NVGRE e VXLAN, bem como encaminhamento de externa com base em VLAN do MAC.  
  
A extensão VFP tem um caminho lento e o caminho rápido para passagem de pacote. O primeiro pacote em um fluxo deve percorrer todos os grupos de regras em cada camada e fazer uma regra de pesquisa que é uma operação cara. No entanto, depois que um fluxo é registrado na tabela de fluxo unificada com uma lista de ações (com base nas regras de correspondência) todos os pacotes subsequentes serão processados com base nas entradas de tabela de fluxo unificada.  
  
Política da HNV é programada pelo agente de host. Cada adaptador de rede da máquina virtual é configurado com um endereço IPv4. Esses são os CAs que serão usados pelas máquinas virtuais para se comunicarem entre si e eles são transportados nos pacotes IP das máquinas virtuais. A HNV encapsula o quadro de autoridade de certificação em um quadro de PA com base nas políticas de virtualização de rede armazenadas no banco de dados do agente de host.  
  
![Arquitetura da HNV](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  
  
Figura 9: Arquitetura da HNV  
  
## <a name="summary"></a>Resumo  
Os datacenters baseados em nuvem podem oferecer diversos benefícios, como escalabilidade aprimorada e melhor utilização de recursos. Para aproveitar esses possíveis benefícios, é necessária uma tecnologia que basicamente trate dos problemas de escalabilidade multilocatário em um ambiente dinâmico. A HNV foi projetada para lidar com essas questões e também para aprimorar a eficiência operacional do datacenter, separando a topologia da rede virtual para a topologia da rede física. Criando um padrão existente, a HNV é executada no datacenter atual e opera com sua infraestrutura existente do VXLAN. Os clientes com a HNV podem agora consolidar seus datacenters em uma nuvem privada ou estender diretamente seus datacenters para o ambiente de um provedor de servidor hospedagem com uma nuvem híbrida.  
  
## <a name="BKMK_LINKS"></a>Consulte também  
Para saber mais sobre HNVv2 consulte os links a seguir:  
  
|Tipo de conteúdo|Referências|  
|----------------|--------------|  
|**Recursos da comunidade**|-   [Blog da arquitetura de nuvem privada](https://blogs.technet.com/b/privatecloud)<br />-Faça perguntas: [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-   [RFC de rascunho de NVGRE](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN - RFC 7348](https://www.rfc-editor.org/info/rfc7348)|  
|**Tecnologias relacionadas**|-Para detalhes técnicos da virtualização de rede do Hyper-V no Windows Server 2012 R2, consulte [detalhes técnicos da virtualização de rede do Hyper-V](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [Controlador de rede](../../../sdn/technologies/network-controller/Network-Controller.md)|  
  


