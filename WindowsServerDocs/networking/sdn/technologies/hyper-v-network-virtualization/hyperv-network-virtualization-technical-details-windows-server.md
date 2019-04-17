---
title: Detalhes técnicos de virtualização de rede do Hyper-V no Windows Server 2016
description: Este tópico fornece informações técnicas sobre a virtualização de rede do Hyper-V no Windows Server 2016
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
ms.openlocfilehash: af2b2a0b151601124bb473c465e7d5a97f2b9150
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Detalhes técnicos de virtualização de rede do Hyper-V no Windows Server 2016

>Aplica-se a: Windows Server 2016

Virtualização de servidor permite várias instâncias de servidor sejam executados simultaneamente em um único host físico; ainda instâncias do servidor são isoladas uns dos outros. Cada máquina virtual essencialmente opera como se fosse o único servidor em execução no computador físico.  
  
Virtualização de rede oferece uma funcionalidade semelhante, no quais várias redes virtuais (possivelmente com endereços IP sobrepostos) executados na mesma infraestrutura de rede física e cada rede virtual opera como se fosse a rede virtual apenas em execução na infraestrutura de rede compartilhada. Figura 1 mostra essa relação.  
  
![Virtualização de servidor versus virtualização de rede](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  
  
Figura 1: Virtualização de servidor versus virtualização de rede  
  
## <a name="hyper-v-network-virtualization-concepts"></a>Conceitos de virtualização de rede do Hyper-V  
Na virtualização de rede Hyper-V (HNV), um cliente ou Locatário é definido como "proprietário" de um conjunto de sub-redes IP que são implantados em uma empresa ou datacenter. Um cliente pode ser uma empresa ou enterprise com vários departamentos ou unidades de negócios em um datacenter particular que exigem o isolamento de rede ou um locatário em um centro de dados públicos que está hospedado por um provedor de serviços. Cada cliente pode ter um ou mais [redes virtuais](#VirtualNetworks) no datacenter e cada virtual rede consiste em um ou mais [Virtual sub-redes](#VirtualSubnets).  
  
Há duas implementações de HNV que estarão disponíveis no Windows Server 2016: HNVv1 e HNVv2.  
  
-   **HNVv1**  
  
    HNVv1 é compatível com o Windows Server 2012 R2 e o System Center 2012 R2 Virtual Machine Manager (VMM). Configuração para HNVv1 depende de gerenciamento WMI e cmdlets do Windows PowerShell (facilitados por meio do System Center VMM) para definir configurações de isolamento e cliente endereço (CA) - rede virtual - mapeamentos de endereço físico (PA) e roteamento. Não há recursos adicionais foram adicionados ao HNVv1 no Windows Server 2016 e nenhum novo recurso planejado.  
    
    • Defina agrupamento e HNV V1 não são compatíveis pela plataforma.
    
    R para usar os usuários de gateways HA NVGRE como usar LBFO equipe ou nenhuma equipe. Ou
    
    Gateways de uso rede controlador implantado o conjunto agrupada switch.

  
-   **HNVv2**  
  
    Um número significativo de novos recursos está incluído no HNVv2 que é implementado usando o Azure Virtual filtragem de plataforma (VFP) encaminhando extensão no Switch Hyper-V. HNVv2 é totalmente integrado com o Microsoft Azure pilha que inclui o novo controlador de rede na pilha de rede definidos Software (SDN).  Política de rede virtual é definida por meio do Microsoft [rede controlador](../../../sdn/technologies/network-controller/Network-Controller.md) usando um RESTful em sentido norte (NB) API e ativadas para um agente de Host por meio de vários SouthBound Intefaces (SBI) incluindo OVSDB. A política de programas de agente de Host na extensão de VFP do Switch Hyper-V onde ela será imposta.  
  
    > [!IMPORTANT]  
    > Este tópico se concentra em HNVv2.  
  
### <a name="VirtualNetworks"></a>Rede virtual  
  
-   Cada rede virtual consiste em uma ou mais sub-redes virtuais. Uma rede virtual forma um limite de isolamento onde as máquinas virtuais em uma rede virtual pode apenas se comunicam entre si. Tradicionalmente, esse isolamento foi aplicado usando VLANs com um intervalo de endereços IP distinguido e 802.1 q marca ou ID de VLAN. Mas, com HNV, isolamento é imposto usando encapsulamento NVGRE ou VXLAN para criar redes de sobreposição com a possibilidade de sobreposição de sub-redes IP entre clientes ou locatários.  
  
-   Cada rede virtual tem uma exclusiva roteamento domínio ID (RDID) no host. Este RDID aproximadamente é mapeado para uma ID de recurso para identificar a recurso REST no controlador de rede de rede virtual. A recurso REST de rede virtual sejam referenciada usando um namespace Uniform Resource Identifier (URI) com a ID do recurso acrescentado.  
  
### <a name="VirtualSubnets"></a>Sub-redes virtuais  
  
-   Uma sub-rede virtual implementa a semântica de sub-rede IP de camada 3 para as máquinas virtuais na mesma sub-rede virtual. A sub-rede virtual formulários um domínio difusão (semelhante obtenham uma) e isolamento é imposto usando o campo o NVGRE locatário rede ID (TNI) ou o identificador de rede VXLAN (VNI).  
  
-   Cada sub-rede virtual pertence a uma única rede virtual (RDID), e ele é atribuído um exclusivo Virtual sub-rede ID (VSID) usando o TNI ou VNI chave no cabeçalho de pacote encapsulado. O VSID deve ser exclusivo em datacenter e está no intervalo de 4096 a 2 ^ 2 24.  
  
A principal vantagem da rede virtual e domínio de roteamento é que ele permite que os clientes trazer seus próprios topologias de rede (por exemplo, sub-redes IP) para a nuvem. Figura 2 mostra um exemplo em que a Contoso Corp tem duas redes separadas, o R & D Net e o Net de vendas. Como essas redes têm domínio roteamento diferente IDs, eles não podem interagir com uns aos outros. Ou seja, Contoso R & D Net é isolado do Contoso vendas Net mesmo que ambos são de propriedade Contoso corp. Contoso R & D Net contém três sub-redes virtuais. Observe que o RDID e VSID são exclusivas dentro de um data center.  
  
![Redes de cliente e subredes virtuais](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  
  
Figura 2: Redes de cliente e subredes virtuais  
  
**Encaminhamento de camada 2**  
  
Na Figura 2, as máquinas virtuais em VSID 5001 podem ter seus pacotes encaminhados para máquinas virtuais que também estejam no VSID 5001 pelo Switch Hyper-V. Os pacotes de entrada de uma máquina virtual em VSID 5001 são enviados para um VPort específico no Switch Hyper-V. Regras de entrada (por exemplo, encap) e mapeamentos (por exemplo, o cabeçalho de encapsulamento) são aplicados pela opção Hyper-V para esses pacotes. Os pacotes são então encaminhados para um VPort diferente em Alternar o Hyper-V (se a máquina virtual de destino está anexada para o mesmo host) ou a outro switch Hyper-V em um host diferente (se a máquina virtual de destino está localizada em um host diferente).  
  
**Roteamento na camada 3**  
  
Da mesma forma, as máquinas virtuais em VSID 5001 podem ter seus pacotes roteados para máquinas virtuais em VSID 5002 ou VSID 5003 pelo roteador distribuído HNV que está presente em VSwitch do cada host Hyper-V. Após a entrega do pacote para o Hyper-V alternar, HNV atualizações VSID do pacote recebido VSID da máquina virtual destino. Isso só acontecerá se ambas as VSIDs estão em RDID o mesmo.  Portanto, os adaptadores de rede virtual com RDID1 não podem enviar pacotes para adaptadores de rede virtual com RDID2 sem percorrendo um gateway.  
  
> [!NOTE]  
> A descrição de fluxo de pacote acima, o termo "máquina virtual" realmente significa o adaptador de rede virtual na máquina virtual. O caso comum é que uma máquina virtual só tem um adaptador de rede virtual. Nesse caso, as palavras "máquina virtual" e "adaptador de rede virtual" podem conceitualmente significa a mesma coisa.  
  
Cada sub-rede virtual define uma sub-rede IP de camada 3 e um limite de domínio de transmissão de camada 2 (L2) semelhante obtenham uma. Quando uma máquina virtual transmite um pacote, HNV usa Unicast replicação (UR) para fazer uma cópia do pacote original e substitua o IP de destino e o MAC os endereços de cada VM que estão no VSID mesmo.  
  
> [!NOTE]  
> Quando o Windows Server 2016 é fornecido, transmissão e sub-rede seletivas serão implementadas usando unicast replicação. Roteamento de multicast entre sub-rede e IGMP não são permitidos.  
  
Além de ser um domínio de transmissão, o VSID fornece isolamento. Um adaptador de rede virtual no HNV está conectado a uma porta de comutador do Hyper-V que terá as regras ACL aplicadas diretamente à porta (recurso REST virtualNetworkInterface) ou para a sub-rede virtual (VSID) do qual ele é uma parte.  
  
A porta do switch Hyper-V deve ter uma regra ACL aplicada. Essa ACL poderia ser permitir tudo, negar tudo, ou ser mais específico para permitir apenas certos tipos de tráfego com base na correspondência de 5 tuplas (IP de origem, IP de destino, porta de origem, porta de destino, protocolo).  
  
> [!NOTE]  
> Extensões de comutador do Hyper-V não funcionará com HNVv2 na nova pilha de rede definidos Software (SDN). HNVv2 é implementado usando a extensão de comutador de plataforma de filtragem virtuais do Azure (VFP) que não pode ser usada em conjunto com qualquer outra extensão de terceiros 3ª switch.  
  
## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Alternar e roteamento na virtualização de rede do Hyper-V  
HNVv2 implementa alternar correto de camada 2 (L2) e a semântica de roteamento Layer 3 (L3) para funcionar apenas como um comutador físico ou roteador funcionaria. Quando uma máquina virtual conectado a uma rede virtual HNV tenta estabelecer uma conexão com outra máquina virtual na mesma virtual sub-rede (VSID)-lo primeiro precisa saber o endereço MAC de CA da máquina virtual remoto. Se houver uma entrada ARP para o endereço IP da máquina virtual destino na tabela de ARP da máquina virtual origem, o endereço MAC dessa entrada é usado. Se uma entrada não existir, a máquina virtual de origem enviará que um ARP transmitida com uma solicitação para o endereço MAC correspondente ao endereço IP da máquina virtual destino a ser retornado. A opção Hyper-V será interceptar essa solicitação e enviá-lo para o agente do Host. O agente de Host examinará seu banco de dados local para um endereço MAC correspondente para o endereço IP da máquina virtual destino solicitado.  
  
> [!NOTE]  
> O agente do Host, atuando como o servidor OVSDB, usa uma variante do esquema VTEP para armazenar os mapeamentos de CA-PA, tabela do MAC e assim por diante.  
  
Se um endereço MAC está disponível, o agente de Host injeta uma resposta ARP e envia essa configuração novamente para a máquina virtual. Após a pilha de rede da máquina virtual tem todas as L2 cabeçalho informações necessárias, o quadro é enviado à porta Hyper-V correspondente a opção V. Internamente, a opção Hyper-V testa esse quadro contra regras de correspondência N tupla atribuído à porta-V e aplica determinadas transformações para o quadro com base nessas regras. Mais importante, um conjunto de transformações de encapsulamento é aplicado para construir o cabeçalho de encapsulamento usando NVGRE ou VXLAN, dependendo da política definida no controlador de rede. Com base na política programada pelo agente do Host, um mapeamento de CA-PA é usado para determinar o endereço IP do host do Hyper-V em que reside a máquina virtual de destino. A opção Hyper-V garante que as regras de roteamento corretas e VLAN marcas são aplicadas ao pacote externo para atingir o endereço PA remoto.  
  
Se uma máquina virtual conectada a uma rede virtual HNV quiser criar uma conexão com uma máquina virtual em uma sub-rede virtual diferente (VSID), o pacote precisa ser encaminhados adequadamente. HNV assume uma topologia de estrela onde há apenas um endereço IP no espaço da autoridade de certificação usado como o próximo salto para alcançar todos os prefixos IP (significado uma rota/gateway padrão). Atualmente, isso reforça uma limitação de uma única rota padrão e rotas não padrão não são suportadas.  
  
### <a name="routing-between-virtual-subnets"></a>Roteamento entre sub-redes Virtual  
Em uma rede física, uma sub-rede IP é um domínio de camada 2 (L2) onde computadores (físicos e virtuais) podem se comunicar diretamente com uns aos outros. O domínio L2 é um domínio difusão onde aprendidas entradas ARP (mapa de endereço IP:MAC) por meio de solicitações de ARP que são transmitidas em todas as interfaces e respostas de ARP são enviadas de volta para o host solicitante. O computador usa as informações de MAC aprendidas a partir da resposta ARP para construir completamente o quadro de L2, incluindo cabeçalhos de Ethernet. No entanto, se for um endereço IP em uma sub-rede L3 diferente, a solicitação ARP não ultrapassar o limite este L3. Em vez disso, uma interface de roteador L3 (próximo salto ou gateway padrão) com um endereço IP na sub-rede origem deve responder a essas solicitações ARP com seu próprio endereço MAC.  
  
Na rede padrão do Windows, um administrador pode criar rotas estáticas e atribuí-los a uma interface de rede. Além disso, um "gateway padrão" geralmente é configurado para ser o endereço IP próximo-salto em uma interface onde os pacotes destinados a rota padrão (0.0.0.0/0) são enviadas. Pacotes são enviados para este gateway padrão, não se existirem nenhuma rotas específicas. Isso normalmente é o roteador para sua rede física.  HNV usa um roteador interno que é parte de cada host e tem uma interface em cada VSID para criar um roteador distribuído para as redes virtuais.  
  
Desde que HNV presume que uma topologia em estrela, o roteador distribuído HNV age como um único gateway padrão para todo o tráfego que está acontecendo entre sub-redes virtuais que fazem parte da mesma rede VSID. O endereço usado como os padrões de gateway padrão para o endereço IP mais baixo na VSID e é atribuído ao roteador HNV distribuído. Este roteador distribuído permite uma maneira muito eficiente para todo o tráfego dentro de uma rede VSID fossem roteados adequadamente porque cada host pode rotear diretamente o tráfego para o host apropriado sem a necessidade de um intermediário.  Isso é especialmente verdadeiro quando duas máquinas virtuais na mesma rede VM mas sub-redes virtuais diferentes estão no mesmo host físico.  Como você verá mais tarde nesta seção, o pacote nunca precisa deixar o host físico.  
  
### <a name="routing-between-pa-subnets"></a>Roteamento entre sub-redes PA  
Em contraste com HNVv1 que alocado para cada sub-rede Virtual (VSID) de um endereço IP PA, HNVv2 agora usa um endereço IP de PA por membro da equipe NIC Switch-Embedded agrupamento (conjunto). A implantação padrão pressupõe uma equipe de dois NIC e atribui endereços IP PA dois por host. Um único host tem IPs PA atribuídos a partir a mesma sub-rede na mesma VLAN lógica do provedor (PA). Dois locatário VMs na mesma sub-rede virtual realmente pode estar localizada em dois hosts diferentes que estão conectados a duas sub-redes lógicas diferentes de provedor. HNV irá construir os cabeçalhos IP externos para o pacote encapsulado com base no mapeamento de CA-PA. No entanto, ele depende da pilha de TCP/IP do host ao ARP do gateway de PA padrão e cria os cabeçalhos de Ethernet externos com base na resposta ARP. Normalmente, essa resposta ARP vem da interface SVI no L3 roteador ou comutador físico em que o host estiver conectado. HNV depende, portanto, o roteador L3 para roteamento os pacotes encapsulados entre sub-redes lógicas provedor / VLANs.  
  
### <a name="routing-outside-a-virtual-network"></a>Roteamento fora de uma rede Virtual  
A maioria das implantações do cliente exigirá comunicação do ambiente HNV aos recursos que não fazem parte do ambiente de HNV. Gateways de virtualização de rede são necessárias para permitir a comunicação entre os dois ambientes. Infraestruturas exigir um Gateway HNV incluem nuvem privada e nuvem híbrida. Basicamente, gateways HNV são necessários para roteamento de camada 3 entre redes (físicos) internas e externas (incluindo NAT) ou entre diferentes sites e/ou nuvens (particulares ou públicos) que usam um túnel IPSec VPN ou GRE.  
  
Gateways podem vir em fatores forma diferentes de física. Eles podem ser criados no Windows Server 2016, incorporadas em um switch de cima de Rack (TOR) atuando como um Gateway VXLAN, acessado por meio de um IP Virtual (VIP) anunciada pelo Balanceador, coloque em outros dispositivos de rede existente, ou pode ser um novo dispositivo de rede autônoma.  
  
Para obter mais informações sobre opções de Gateway de RAS do Windows, consulte [RAS Gateway](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="packet-encapsulation"></a>Encapsulamento de pacote  
Cada adaptador de rede virtual no HNV está associado com dois endereços IP:  
  
-   **Endereço do cliente** (CA) o endereço IP atribuído pelo cliente, com base em sua infraestrutura de intranet. Esse endereço permite que o cliente trocar o tráfego de rede com a máquina virtual como se ele não tinha sido movido para uma nuvem pública ou privada. A CA é visível para a máquina virtual e acessível pelo cliente.  
  
-   **Endereço do provedor** (PA) o endereço IP atribuída pelo provedor de hospedagem ou os administradores de datacenter com base em sua infraestrutura de rede física. O PA aparece nos pacotes de rede que são trocados com o servidor que executa o Hyper-V que hospeda a máquina virtual. O PA está visível na rede física, mas não para a máquina virtual.  
  
As CAs manter uma topologia de rede do cliente, que é virtualizada e dissociada da topologia de rede física subjacente real e endereços, conforme implementada pelo PAs. O diagrama a seguir mostra a relação entre a máquina virtual CAs e infraestrutura de rede PAs como resultado de virtualização de rede conceitual.  
  
![Diagrama conceitual de virtualização de rede sobre infraestrutura física](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  
  
Figura 6: Diagrama conceitual de virtualização de rede sobre infraestrutura física  
  
No diagrama, máquinas virtuais de cliente está enviando pacotes de dados no espaço da autoridade de certificação, que percorram a infraestrutura de rede física por meio de suas próprias redes virtuais ou "túneis". No exemplo acima, os túneis podem ser pensados como "envelopes" ao redor os pacotes de dados Contoso e Fabrikam com rótulos verdes para entrega (PA endereços) seja entregue do host de origem à esquerda para o host de destino à direita. A chave é como os hosts determinam os "endereços para entrega" (PA) correspondente a Contoso e a autoridade de certificação de Fabrikam, como "envelope" é colocado em torno os pacotes e como os hosts de destino podem decodificar os pacotes e fornecer para as máquinas de virtuais de destino Contoso e Fabrikam corretamente.  
  
Essa analogia simple realçado os principais aspectos da virtualização de rede:  
  
-   Cada máquina virtual CA é mapeada para um host físico PA. Pode haver várias autoridades de certificação associados com o mesmo PA.  
  
-   Máquinas virtuais enviar pacotes de dados em espaços CA, que são colocados em um "envelope" com um par de origem e destino PA com base no mapeamento.  
  
-   Os mapeamentos de CA-PA devem permitir que os hosts diferenciar os pacotes para máquinas virtuais de cliente diferente.  
  
Como resultado, o mecanismo para virtualizar a rede é virtualizar os endereços de rede usados por máquinas virtuais. O controlador de rede é responsável pelo mapeamento de endereço e o agente de host mantém o banco de dados de mapeamento usando o esquema MS_VTEP. A próxima seção descreve o mecanismo de virtualização de endereço real.  
  
## <a name="network-virtualization-through-address-virtualization"></a>Virtualização de rede por meio da virtualização de endereço  
Implementa HNV sobreponha redes de locatário usando rede virtualização genérico de roteamento de encapsulamento (NVGRE) ou Virtual eXtensible rede Local (VXLAN).  VXLAN é o padrão.  
  
### <a name="virtual-extensible-local-area-network-vxlan"></a>Rede Local virtual eXtensible (VXLAN)  
Virtual eXtensible rede Local (VXLAN) ([RFC 7348](http://www.rfc-editor.org/info/rfc7348)) protocolo tem sido adotado no mercado, com suporte de fornecedores como Cisco, Brocade, Arista, Dell, HP e outros. O protocolo VXLAN usa UDP como o transporte. A porta UDP atribuído IANA de destino para VXLAN é 4789 e a porta UDP de origem deve ser um hash de informações de pacote a ser usado para ECMP distribuindo interno. Após o cabeçalho UDP, um cabeçalho VXLAN é acrescentado ao pacote que inclui um campo de 4 bytes reservado seguido por um campo de 3 bytes para o VXLAN rede identificador (VNI) - VSID - seguido por outro campo 1 byte reservado. Após o cabeçalho VXLAN, é acrescentado ao quadro CA L2 original (sem o quadro de Ethernet CA FCS).  
  
![Cabeçalho do pacote VXLAN](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  
  
### <a name="generic-routing-encapsulation-nvgre"></a>Encapsulamento de roteamento genérico (NVGRE)  
Esse mecanismo de virtualização de rede usa o genérico de roteamento de encapsulamento (NVGRE) como parte do cabeçalho de encapsulamento. Em NVGRE, pacote da máquina virtual é encapsulado em outro pacote. O cabeçalho desse novo pacote tem a origem apropriada e endereços IP PA além do ID de sub-rede Virtual, que é armazenada no campo de chave do cabeçalho GRE, conforme mostrado na Figura 7.  
  
![Encapsulamento de NVGRE](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  
  
Figura 7: Virtualização de rede - encapsulamento NVGRE  
  
A identificação de sub-rede Virtual permite que hosts identificar a máquina virtual de cliente para qualquer pacote específico, mesmo que o PA e a CA nos pacotes podem se sobrepor. Isso permite que todas as máquinas virtuais no mesmo host para compartilhar um único PA, conforme mostrado na Figura 7.  
  
Compartilhar o PA tem um grande impacto na escalabilidade de rede. O número de endereços IP e MAC que precisam ser aprendeu com a infraestrutura de rede pode ser consideravelmente reduzido. Por exemplo, se cada host final tem uma média de 30 máquinas de virtuais, o número de IP e endereços MAC que precisam ser aprendeu com a infraestrutura de rede é reduzido por um fator de 30. as IDs de sub-rede Virtual inseridos nos pacotes de também habilite correlação fácil de pacotes para os clientes reais.  
  
O PA compartilhamento esquema para o Windows Server 2012 R2 é um PA por VSID por host. Para Windows Server 2016 o esquema é um PA por membro da equipe de placa de rede.  
  
Com o Windows Server 2016 e posterior, HNV totalmente compatível com NVGRE e VXLAN prontamente; ele não requer a atualização ou compra de novo hardware de rede como NICs (adaptadores de rede), comutadores ou roteadores. Isso ocorre porque esses pacotes na conexão são pacotes IP normal no espaço de PA, que é compatível com a infraestrutura de rede de hoje.  No entanto obter o melhor uso de desempenho suporte NICs com os drivers mais recentes que dão suporte a tarefa descarrega.  
  
## <a name="multi-tenant-deployment-example"></a>Exemplo de implantação de vários locatários  
O diagrama a seguir mostra um exemplo de implantação de dois clientes localizados em um datacenter na nuvem com a relação de CA-PA definida pelas políticas de rede.  
  
![Exemplo de implantação de vários locatários](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  
  
Figura 8: Exemplo de implantação de vários locatários  
  
Considere o exemplo na Figura 8. Antes de mover para o provedor de hospedagem do serviço de IaaS compartilhado:  
  
-   Contoso Corp executou um SQL Server (denominado **SQL**) em IP abordam 10.1.1.11 e um servidor web (denominado **Web**) no endereço IP 10.1.1.12, que usa o SQL Server para transações de banco de dados.  
  
-   Fabrikam Corp executou um SQL Server, também denominada **SQL** e atribuído o endereço IP 10.1.1.11 e um servidor web, também denominada **Web** e também no endereço IP 10.1.1.12, que usa o SQL Server para transações de banco de dados.  
  
Supomos que o provedor de serviços de hospedagem tenha criado anteriormente rede lógico do provedor (PA) por meio do controlador de rede para corresponder a sua topologia de rede física. O controlador de rede aloca dois endereços IP PA de prefixo IP da sub-rede lógica onde os hosts estão conectados. O controlador de rede também indica a marca VLAN apropriada para aplicar os endereços IP.  
  
Usando o controlador de rede, Corp de Contoso e Fabrikam Corp, em seguida, crie sua rede virtual e subredes que são apoiadas por rede lógico do provedor (PA) especificada pelo provedor de serviço de hospedagem. Corp Contoso e Fabrikam Corp movem seus respectivos servidores SQL e servidores web para o provedor de hospedagem mesmo compartilhado IaaS serviço onde, Coincidentemente, eles são executados os **SQL** máquinas virtuais em 1 de Host do Hyper-V e o **Web** máquinas de virtuais (IIS7) em 2 de Host do Hyper-V. Todas as máquinas virtuais manter seus endereços IP da intranet originais (suas CAs).  
  
Ambas as empresas são atribuídas a seguir Virtual sub-rede ID (VSID) com o controlador de rede, conforme indicado a seguir.  O agente de Host em cada um dos hosts Hyper-V recebe os endereços IP PA alocados do controlador de rede e cria dois PA host vNICs em um compartimento de rede não padrão. Uma interface de rede é atribuída a cada um desses vNICs host em que o endereço IP PA é atribuído como mostrado a seguir:  
  
-   Máquinas virtuais da Contoso Corp VSID e PAs: **VSID** é 5001, **SQL PA** for 192.168.1.10, **Web PA** é 192.168.2.20  
  
-   Máquinas virtuais da Fabrikam Corp VSID e PAs: **VSID** é 6001, **SQL PA** for 192.168.1.10, **Web PA** é 192.168.2.20  
  
O controlador de rede plumbs todos os política de rede (incluindo o mapeamento de CA-PA) para o agente de Host SDN que manterá a política em um armazenamento persistente (em tabelas de banco de dados OVSDB).  
  
Quando a máquina virtual Contoso Corp Web (10.1.1.12) em 2 de Host do Hyper-V cria uma conexão TCP para o SQL Server em 10.1.1.11, ocorre o seguinte:  
  
-   VM ARPs o endereço MAC de destino do 10.1.1.11  
  
-   A extensão VFP em vSwitch intercepta esse pacote e o envia para o agente de Host SDN  
  
-   O agente de Host SDN parece em seu repositório de política para o endereço MAC do 10.1.1.11  
  
-   Se for encontrado um MAC, o agente de Host injeta uma resposta ARP volta para a VM  
  
-   Se não for encontrado um MAC, nenhuma resposta será enviada e a entrada ARP na VM para 10.1.1.11 estiver marcada inacessível.  
  
-   A VM agora constrói um pacote TCP com os cabeçalhos de CA Ethernet e IP corretos e o envia para o vSwitch  
  
-   O VFP encaminhando extensão no vSwitch processa esse pacote pelas camadas VFP (descrito abaixo) atribuída à porta vSwitch origem em que o pacote foi recebido e cria uma nova entrada de fluxo na tabela VFP fluxo unificado  
  
-   O mecanismo VFP realiza pesquisa de correspondência ou tabela do fluxo de regra para cada camada (por exemplo, a camada de rede virtual) com base em cabeçalhos de IP e Ethernet.  
  
-   A regra correspondente na camada de rede virtual faz referência a um espaço de mapeamento de CA-PA e executa o encapsulamento.  
  
-   O tipo de encapsulamento (VXLAN ou NVGRE) é especificado na camada VNet juntamente com o VSID.  
  
-   No caso de encapsulamento VXLAN, um cabeçalho UDP externo é construído com VSID de 5001 no cabeçalho VXLAN.  
    Um cabeçalho IP externo é construído com o endereço de PA de origem e destino atribuído para o Host do Hyper-V 2 (192.168.2.20) e 1 de Host do Hyper-V (192.168.1.10) respectivamente com base no repositório de políticas do agente de Host SDN.  
  
-   Esse pacote depois fluem para a camada de roteamento PA em VFP.  
  
-   A camada de roteamento PA em VFP será referenciar compartimento da rede usado para o tráfego PA-espaço e um ID de VLAN e usar a pilha de TCP/IP do host para encaminhar o pacote de PA como 1 de Host do Hyper-V corretamente.  
  
-   Ao receber o pacote encapsulado, 1 de Host do Hyper-V recebe o pacote no compartimento da rede PA e encaminhá-lo ao vSwitch.  
  
-   O VFP processa o pacote por meio de suas camadas VFP e crie uma nova entrada de fluxo na tabela VFP fluxo unificado.  
  
-   O mecanismo de VFP coincida com as regras de ingres na camada de rede virtual e corta cabeçalhos de Ethernet, IP e VXLAN do pacote encapsulado externa.  
  
-   O mecanismo VFP encaminha o pacote à porta vSwitch para que o destino VM está conectado.  
  
Um processo semelhante para o tráfego entre o Corp Fabrikam **Web** e **SQL** máquinas virtuais usa as configurações de política HNV para a Fabrikam corp Como resultado, com máquinas virtuais HNV, Fabrikam Corp e Contoso Corp interagir como se estivessem em suas intranets originais. Eles nunca podem interagir com os outros, mesmo que eles estão usando os mesmos endereços IP.  
  
Os endereços separados (CAs e PAs), as configurações de política dos hosts Hyper-V e a conversão de endereço entre a CA e PA para o tráfego de entrada e saída máquina virtual isolam desses conjuntos de servidores usando a chave NVGRE ou o VLXAN VNID. Além disso, os mapeamentos de virtualização e transformação separa os a arquitetura de rede virtual da infraestrutura de rede física. Embora Contoso **SQL** e **Web** e Fabrikam **SQL** e **Web** residir em suas próprias sub-redes IP CA (10.1.1/24), sua implantação física acontece em dois hosts em sub-redes PA diferentes, 192.168.1/24 e 192.168.2/24, respectivamente. A implicação é cruzado sub-rede provisionamento e máquinas virtuais migração em tempo real se tornam possíveis com HNV.  
  
## <a name="hyper-v-network-virtualization-architecture"></a>Arquitetura de virtualização de rede do Hyper-V  
No Windows Server 2016, HNVv2 é implementado usando o Azure Virtual filtragem de plataforma (VFP) que é uma extensão de filtragem NDIS dentro do Switch Hyper-V. O principal conceito do VFP é de um mecanismo de fluxo de ação de correspondência com uma API interno exposto para o agente de Host SDN para programar a política de rede. O agente de Host SDN em si recebe a política de rede do controlador de rede sobre os canais de comunicação OVSDB e WCF SouthBound. Não só é a política de rede virtual (por exemplo, o mapeamento de CA-PA) programados usando VFP mas política adicional como ACLs, QoS e assim por diante.  
  
A hierarquia de objetos para o vSwitch e VFP encaminhando extensão é o seguinte:  
  
-   vSwitch  
  
    -   Gerenciamento de NIC externa  
  
    -   Descarrega de Hardware da placa de rede  
  
    -   Regras de encaminhamento globais  
  
    -   Porta  
  
        -   Encaminhamento de camada para a fixação de cabelo de saída  
  
        -   Listas de espaço para mapeamentos e pools NAT  
  
        -   Tabela de fluxo unificado  
  
        -   Camada VFP  
  
            -   Tabela de fluxo  
  
            -   Grupo  
  
            -   Regra  
  
                -   As regras podem referenciar espaços  
  
Em VFP, uma camada é criada por tipo de política (por exemplo, rede Virtual) e é um conjunto genérico de tabelas de regra/fluxo. Ele não tem nenhuma funcionalidade intrínseca até regras específicas são atribuídas a essa camada de implementar essa funcionalidade. Cada camada é atribuída uma prioridade e camadas são atribuídas a uma porta por prioridade em ordem crescente. As regras são organizadas em grupos com base principalmente na direção e a família de endereço IP. Grupos também recebem uma prioridade e no máximo, uma regra de um grupo pode corresponder a um determinado fluxo.  
  
A lógica de encaminhamento de vSwitch com extensão VFP é o seguinte:  
  
-   Processamento de entrada (ingresso do ponto de vista do pacote chegando em uma porta)  
  
-   Encaminhamento  
  
-   Processamento de saída (do ponto de vista do pacote deixar uma porta de saída)  
  
O VFP dá suporte interno encaminhamento MAC para tipos de encapsulamento NVGRE e VXLAN, bem como externo encaminhamento VLAN MAC com base.  
  
A extensão VFP tem um caminho lento e o caminho rápido para passagem de pacote. O primeiro pacote em um fluxo deve percorrer todos os grupos de regras de cada camada e fazer uma regra de pesquisa que é uma operação cara. No entanto, quando um fluxo é registrado na tabela unificado de fluxo com uma lista de ações (com base nas regras de correspondência) todos os pacotes subsequentes serão processados com base nas entradas de tabela do fluxo unificado.  
  
Política HNV está programada pelo agente do host. Cada adaptador de rede de máquina virtual é configurado com um endereço IPv4. Essas são as autoridades de certificação que serão usado por máquinas virtuais para se comunicar uns com os outros, e eles são carregados em pacotes IP de máquinas virtuais. HNV encapsula o quadro de autoridade de certificação em um quadro PA com base em políticas de virtualização de rede armazenadas no banco de dados do agente do host.  
  
![Arquitetura HNV](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  
  
Figura 9: HNV arquitetura  
  
## <a name="summary"></a>Resumo  
Os datacenters baseado em nuvem pode fornecer muitos benefícios como escalabilidade aprimorada e melhor utilização de recursos. Para obter esses benefícios potenciais requer uma tecnologia que fundamentalmente aborda os problemas de escalabilidade de vários locatário em um ambiente dinâmico. HNV foi projetado para solucioná-los e também melhorar a eficiência do data center por dissociação a topologia de rede virtual para a topologia de rede física. Com base em um padrão existente, HNV é executado no datacenter de hoje e opera com sua infraestrutura VXLAN existente. Os clientes com HNV podem consolidar agora seus centros de dados em uma nuvem privada ou estender perfeitamente seus datacenters para o ambiente do provedor um servidor host com uma nuvem híbrida.  
  
## <a name="BKMK_LINKS"></a>Consulte também  
Para saber mais sobre HNVv2, consulte os links a seguir:  
  
|Tipo de conteúdo|Referências|  
|----------------|--------------|  
|**Recursos da comunidade**|-   [Blog da arquitetura de nuvem privada](http://blogs.technet.com/b/privatecloud)<br />-Perguntas:[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-   [Rascunho NVGRE RFC](http://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN - RFC 7348](http://www.rfc-editor.org/info/rfc7348)|  
|**Tecnologias relacionadas**|-Para detalhes técnicos de virtualização de rede do Hyper-V no Windows Server 2012 R2, consulte [detalhes técnicos da virtualização de rede do Hyper-V](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [Controlador de rede](../../../sdn/technologies/network-controller/Network-Controller.md)|  
  


