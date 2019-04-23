---
title: Visão geral da virtualização de rede Hyper-V no Windows Server 2016
description: Este tópico fornece uma visão geral da virtualização de rede do Hyper-V no Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0115b7ad-d229-4c69-9d7e-a3f5fbaa3b2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aecc66630d8fbe994b6619424fa26b5c0c8d0921
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862187"
---
# <a name="hyper-v-network-virtualization-overview-in-windows-server-2016"></a>Visão geral da virtualização de rede Hyper-V no Windows Server 2016

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

No Windows Server 2016 e no Virtual Machine Manager, a Microsoft fornece uma solução de virtualização de rede de ponta a ponta.  Há cinco componentes principais que compõem a solução de virtualização de rede da Microsoft:  
  
-   **Windows Azure Pack for Windows Server** oferece um para o portal para criar redes virtuais e um portal administrativo para gerenciar redes virtuais de locatários.  
  
-   **Virtual Machine Manager** (VMM) oferece gerenciamento centralizado da malha de rede.  
  
-   **Controlador de rede Microsoft** fornece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede física e virtual em seu datacenter.  
  
-   A**Virtualização de Rede Hyper-V** oferece a infraestrutura necessária para virtualizar o tráfego da rede.  
  
-   **Gateways de virtualização de rede do Hyper-V** fornecem conexões entre redes virtuais e físicas.  
  
Este tópico apresenta os conceitos e explica os principais benefícios e recursos de virtualização de rede do Hyper-V (parte da solução geral de virtualização de rede) no Windows Server 2016. Ele explica como a virtualização de rede beneficia as nuvens privadas para consolidação de carga de trabalho corporativa e os provedores de serviço de nuvem pública de IaaS (Infraestrutura como Serviço).  
  
Para obter mais detalhes técnicos sobre a virtualização de rede no Windows Server 2016, consulte [rede do Hyper-V detalhes técnicos da virtualização no Windows Server 2016](../../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md).  
  
**Você quis dizer**  
  
-   [Visão geral da virtualização de rede Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)  
  
-   [Visão geral do Hyper-V](assetId:///5aad349f-ef06-464a-b36f-366fbb040143)  
  
-   [Visão geral do Comutador Virtual do Hyper-V](assetId:///e6ec46af-6ef4-49b3-b1f1-5268dc03f05b)  
  
## <a name="BKMK_OVER"></a>Descrição do recurso  
Virtualização de rede do Hyper-V fornece "redes virtuais" (chamados de uma rede VM) para máquinas virtuais semelhantes àquela como a virtualização de servidor (hipervisor) fornece "máquinas virtuais" para o sistema operacional. A virtualização de rede separa redes virtuais da infraestrutura de rede física e remove as restrições da atribuição de endereços IP hierárquicos e de VLAN do provisionamento de máquinas virtuais. Essa flexibilidade facilita a migração de clientes para nuvens IaaS e torna mais eficiente o gerenciamento da infraestrutura pelos hosters e administradores do datacenter, mantendo ainda o isolamento multilocatário, os requisitos de segurança e o suporte à sobreposição de endereços IP de máquinas virtuais.  
  
Os clientes desejam estender diretamente seus datacenters para a nuvem. Atualmente, existem desafios técnicos tornar essas arquiteturas de nuvem híbrida perfeita. Um dos maiores obstáculos enfrentados pelos consumidores é reutilizar suas topologias de rede existentes (sub-redes IP endereços, serviços de rede e assim por diante) na nuvem e ponte entre seus recursos locais e seus recursos de nuvem.  A Virtualização de Rede Hyper-V oferece o conceito de uma Rede VM independente da rede física subjacente. Com esse conceito de Rede VM, composta de uma ou mais Sub-redes Virtuais, a localização física exata da rede física das máquinas virtuais ligadas a uma rede virtual é separada da topologia da rede virtual. Como resultado, os clientes podem mover facilmente suas sub-redes virtuais para a nuvem preservando seus endereços IP e sua topologia existente na nuvem, de forma que os serviços existentes continuam funcionando independentemente da localização física das sub-redes. Ou seja, a Virtualização de Rede Hyper-V permite uma nuvem híbrida integrada.  
  
Além da nuvem híbrida, muitas organizações estão consolidando seus datacenters e criando nuvens privadas para obter internamente o benefício das arquiteturas em nuvem em termos de eficiência e escalabilidade. Virtualização de rede do Hyper-V permite melhor flexibilidade e eficiência para nuvens privadas, separando a uma unidade de negócios da topologia de rede (tornando-o virtual) da topologia da rede física real. Assim, as unidades de negócios podem facilmente compartilhar uma nuvem privada interna, ao mesmo tempo em que ficam isoladas umas das outras e continuam a manter as topologias de rede existentes. A equipe de operações do datacenter tem flexibilidade para implantar e mover dinamicamente cargas de trabalho em qualquer lugar do datacenter sem interrupções do servidor, fornecendo eficiências operacionais melhores e, no geral, um datacenter mais eficiente.  
  
Para os proprietários da carga de trabalho, o principal benefício é que agora eles podem migrar sua "topologias" de carga de trabalho para a nuvem sem alterar seus endereços IP ou reescrever seus aplicativos. Por exemplo, o aplicativo LOB típico de três camadas é composto de uma camada de front-end, uma camada de lógica de negócios e uma camada de banco de dados. Por meio da política, a Virtualização de Rede Hyper-V permite que o cliente carregue todas as partes das três camadas na nuvem, mantendo a topologia de roteamento e os endereços IP dos serviços (ou seja, os endereços IP da máquina virtual), sem a necessidade de alterar os aplicativos.  
  
Para proprietários de infraestrutura, a flexibilidade adicional no posicionamento de máquinas virtuais torna possível mover cargas de trabalho em qualquer lugar dos datacenters sem alterar o as máquinas virtuais ou reconfigurar as redes. Por exemplo, a Virtualização de Rede Hyper-V permite a migração ao vivo entre sub-redes, de forma que uma máquina virtual pode migrar ao vivo em qualquer lugar do datacenter sem uma interrupção do serviço. Anteriormente, a migração ao vivo era limitada à mesma sub-rede, restringindo os locais onde as máquinas virtuais podiam ficar localizadas. A migração ao vivo entre sub-redes permite que os administradores consolidem cargas de trabalho com base em requisitos de recursos dinâmicos, eficiência de energia e também possam acomodar a manutenção da infraestrutura sem interromper o tempo de atividade da carga de trabalho do cliente.  
  
## <a name="BKMK_APP"></a>Aplicativos práticos  
Com o sucesso dos datacenters virtualizados, as organizações de TI e os provedores de hospedagem (provedores que fornecem aluguéis de servidores físicos ou colocação) começaram a oferecer infraestruturas virtualizadas mais flexíveis que facilitam a oferta de instâncias de servidor por demanda a seus clientes. Essa nova classe de serviços é chamada de IaaS (Infraestrutura como Serviço). Windows Server 2016 fornece todas as funcionalidades de plataforma necessárias para permitir que clientes corporativos criem nuvens privadas e façam a transição para um IT como um modelo operacional de serviço. Windows Server 2016 2016 também permite que os hosters criem nuvens públicas e ofereçam soluções IaaS a seus clientes. Quando combinado com o Virtual Machine Manager e o pacote do Windows Azure para gerenciar a política de virtualização de rede do Hyper-V, a Microsoft fornece uma poderosa solução de nuvem.  
  
Virtualização de rede do Windows Server 2016 Hyper-V fornece virtualização de rede baseado em políticas, controlada por software que reduz a sobrecarga que as empresas enfrentam ao expandir nuvens IaaS dedicadas de gerenciamento e fornece aos hosters em nuvem melhor flexibilidade e escalabilidade para gerenciar máquinas virtuais para alcançar maior utilização de recursos.  
  
Um cenário IaaS que tem máquinas virtuais de diferentes divisões organizacionais (nuvem dedicada) ou diferentes clientes (nuvem hospedada) exige um isolamento seguro. A solução atual, as redes locais virtuais (VLANs), pode apresentar desvantagens significativas nesse cenário.  
  
**VLANs**  
  
Atualmente, as VLANs são o mecanismo que a maioria das organizações usa para dar suporte à reutilização de espaço de endereço e o isolamento de locatários. Uma VLAN usa a marcação explícita (VLAN ID) nos cabeçalhos de quadros Ethernet e depende dos comutadores Ethernet para impor o isolamento e restringir o tráfego aos nós da rede com a mesma ID da VLAN. As principais desvantagens das VLANs são as descritas a seguir:  
  
-   Risco aumentado de uma interrupção inadvertida devido à trabalhosa reconfiguração de comutadores de produção sempre que máquinas virtuais ou os limites de isolamento se movem no datacenter dinâmico.  
  
-   Escalabilidade limitada porque existe um máximo de 4094 VLANS e os comutadores típicos não dão suporte a mais de 1000 IDs de VLAN.  
  
-   Limitadas a uma única sub-rede de IPs, o que restringe o número de nós em uma única VLAN e o posicionamento de máquinas virtuais com base em localizações físicas. Embora as VLANs possam ser expandidas entre sites, a VLAN inteira deve estar na mesma sub-rede.  
  
**Atribuição de endereço IP**  
  
Além das desvantagens apresentadas pelas VLANs, a atribuição de endereço IP de máquina virtual apresenta problemas, que incluem:  
  
-   As localizações físicas na infraestrutura de rede do datacenter determinam os endereços IP das máquinas virtuais. Como resultado, a mudança para a nuvem normalmente exige a alteração dos endereços IP das cargas de trabalho de serviços.  
  
-   As políticas são associadas a endereços IP, como regras de firewall, descoberta de recursos e serviços de diretório etc. A alteração de endereços IP exige a atualização de todas as políticas associadas.  
  
-   A implantação de máquinas virtuais e o isolamento do tráfego dependem da topologia.  
  
Quando os administradores de rede do datacenter planejam o layout físico do datacenter, devem tomar decisões sobre onde as sub-redes serão posicionadas e roteadas fisicamente. Essas decisões se baseiam na tecnologia de IP e Ethernet, que afeta os possíveis endereços IP permitidos para máquinas virtuais em execução em um determinado servidor ou um blade conectado a um rack específico no datacenter. Quando uma máquina virtual é provisionada e posicionada no datacenter, ela deve estar de acordo com essas opções e restrições referentes ao endereço IP. Portanto, normalmente, os administradores do datacenter atribuem endereços IP novos às máquinas virtuais.  
  
O problema desse requisito é que, além de ser um endereço, há informações semânticas associadas a um endereço IP. Por exemplo, uma sub-rede pode conter determinados serviços ou estar em uma localização física distinta. É comum que regras de firewall, políticas de controle de acesso e associações de segurança IPsec estejam associados a endereços IP. A alteração dos endereços IP exige que os proprietários das máquinas virtuais ajustem todas as políticas baseadas no endereço IP original. Essa sobrecarga com a renumeração é tão grande que muitas empresas optam por implantar somente serviços novos na nuvem, abandonando os aplicativos herdados.  
  
A Virtualização de Rede Hyper-V separa as redes virtuais das máquinas virtuais do cliente da infraestrutura de rede física. Como resultado, as máquinas virtuais do cliente podem manter seus endereços IP originais, e os administradores do datacenter podem provisionar as máquinas virtuais do cliente em qualquer lugar do datacenter sem precisar reconfigurar endereços IP físicos ou IDs da VLAN. A próxima seção resume as principais funcionalidades.  
  
## <a name="BKMK_NEW"></a>Funcionalidade importante  
A seguir está uma lista das principais funcionalidade, benefícios, os recursos da virtualização de rede do Hyper-V no Windows Server 2016:  
  
-   **Habilita o posicionamento da carga de trabalho flexível - o isolamento de rede e endereço IP novamente usar sem VLANs**  
  
    Virtualização de rede do Hyper-V separa a redes virtuais do cliente da infraestrutura de rede física dos hosters, oferecendo liberdade para o posicionamento da carga de trabalho dentro dos datacenters. O posicionamento da carga de trabalho de máquinas virtuais não é mais limitado pelos requisitos de atribuição de endereços IP ou isolamento da VLAN da rede física, pois é imposto com hosts Hyper-V baseados em políticas de virtualização multilocatário definidas pelo software.  
  
    Agora as máquinas virtuais de diferentes clientes com endereços IP sobrepostos podem ser implantadas no mesmo servidor host, sem exigir a trabalhosa configuração da VLAN ou violar a hierarquia de endereços IP. Isso pode agilizar a migração de cargas de trabalho do cliente em provedores de hospedagem IaaS compartilhados, permitindo que os clientes migrem essas cargas de trabalho sem modificações, inclusive deixando os endereços IP das máquinas virtuais inalterados. Para o provedor de hospedagem, o suporte a numerosos clientes que desejam estender seus espaços de endereços de rede existentes para o datacenter IaaS compartilhado é um exercício complexo de configuração e manutenção de VLANs isoladas para cada cliente, a fim de garantir a coexistência de espaços de endereços possivelmente sobrepostos. Com a Virtualização de Rede Hyper-V, o suporte a endereços sobrepostos é facilitado e exige menos reconfiguração da rede pelo provedor de hospedagem.  
  
    Além disso, a manutenção e as atualizações da infraestrutura física podem ser feitas sem causar um tempo de inatividade das cargas de trabalho do cliente. Com a Virtualização de Rede Hyper-V, as máquinas virtuais em um determinado host, rack, sub-rede, VLAN ou em todo o cluster podem ser migradas sem exigir a alteração do endereço IP físico ou uma reconfiguração de grande porte.  
  
-   **Habilita migrações mais fáceis para cargas de trabalho para uma nuvem IaaS compartilhada**  
  
    Com a Virtualização de Rede Hyper-V, as configurações de endereços IP e máquinas virtuais permanecem inalteradas. Isso permite que as organizações de TI migrem mais facilmente as cargas de trabalho de seus datacenters para um provedor de hospedagem IaaS compartilhado, com reconfiguração mínima da carga de trabalho ou das ferramentas e políticas de sua infraestrutura. Nos casos em que há conectividade entre dois datacenters, os administradores de TI podem continuar a usar suas ferramentas sem precisar reconfigurá-las.  
  
-   **Permite a migração ao vivo entre sub-redes**  
  
    Migração ao vivo de cargas de trabalho de máquina virtual tradicionalmente foi limitada à mesma sub-rede IP ou VLAN porque o cruzamento de sub-redes, sistema de operacional de convidado da máquina virtual para alterar seu endereço IP necessário. Essa alteração de endereço interrompe a comunicação existente e os serviços em execução na máquina virtual. Com a virtualização de rede do Hyper-V, as cargas de trabalho podem ser ao vivo migrados de servidores que executam o Windows Server 2016 em uma sub-rede para servidores que executam o Windows Server 2016 em uma sub-rede diferente sem alterar os endereços IP de carga de trabalho. A Virtualização de Rede Hyper-V garante que as alterações de localização das máquinas virtuais devido à migração ao vivo sejam atualizadas e sincronizadas entre os hosts que têm comunicação contínua com as máquinas virtuais migradas.  
  
-   **Permite o gerenciamento mais fácil de administração de servidor e rede separado**  
  
    O posicionamento da carga de trabalho do servidor é simplificado porque a migração e o posicionamento de cargas de trabalho são independentes das configurações da rede física subjacente. Os administradores de servidores podem se concentrar no gerenciamento de serviços e servidores, e os administradores de rede podem se concentrar no gerenciamento geral do tráfego e da infraestrutura de rede. Assim, os administradores de servidores do datacenter podem implantar e migrar máquinas virtuais sem precisar alterar os endereços IP das máquinas virtuais. As sobrecarga é reduzida porque a Virtualização de Rede Hyper-V permite que o posicionamento de máquinas virtuais ocorra de maneira independente da topologia de rede, reduzindo a necessidade de os administradores de rede se envolverem com os posicionamentos que podem alterar os limites do isolamento.  
  
-   **Simplifica a rede e melhora a utilização de recursos de servidor/rede**  
  
    A rigidez das VLANs e a dependência do posicionamento de máquinas virtuais em uma infraestrutura de rede física resulta no superprovisionamento e na subutilização. Com a quebra dessa dependência, a maior flexibilidade do posicionamento da carga de trabalho de máquinas virtuais pode simplificar o gerenciamento de rede e melhorar a utilização de recurso de servidor e de rede. Observe que a Virtualização de Rede Hyper-V dá suporte a VLANs no contexto do datacenter físico. Por exemplo, um datacenter pode requerer que todo o tráfego da Virtualização de Rede Hyper-V esteja em uma VLAN específica.  
  
-   **É compatível com a infraestrutura existente e a tecnologia emergente**  
  
    Virtualização de rede do Hyper-V pode ser implantada no datacenter atual, ainda é compatível com o data center "rede simples" tecnologias emergentes.  
  
    Por exemplo, a HNV no Windows Server 2016 dá suporte ao formato de encapsulamento VXLAN e vSwitch aberto protocolo de gerenciamento de banco de dados (OVSDB) como a Interface SouthBound (SBI)...  
  
-   **Fornece para interoperabilidade e prontidão do ecossistema**  
  
    A Virtualização de Rede Hyper-V dá suporte a várias configurações de comunicação com recursos existentes, como conectividade entre locais, SAN (rede de área de armazenamento), acesso a recursos não virtualizados etc. A Microsoft tem o compromisso de trabalhar com parceiros de ecossistema para dar suporte e aperfeiçoar a experiência da Virtualização de Rede Hyper-V em termos de desempenho, escalabilidade e gerenciabilidade.  
  
-   **Configuração baseada em política**  
  
    Políticas de virtualização de rede no Windows Server 2016 são configuradas por meio do controlador de rede da Microsoft. O controlador de rede tem uma API RESTful northbound e a interface do Windows PowerShell para configurar a política. Para obter mais informações sobre o controlador de rede da Microsoft, consulte [controlador de rede](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md).  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Virtualização de rede do Hyper-V usando o controlador de rede da Microsoft requer o Windows Server 2016 e a função Hyper-V.  
  
## <a name="BKMK_LINKS"></a>Consulte também  
Para saber mais sobre a virtualização de rede do Hyper-V no Windows Server 2016, consulte os links a seguir:  
  
|Tipo de conteúdo|Referências|  
|----------------|--------------|  
|**Recursos da comunidade**|-   [Blog da arquitetura de nuvem privada](https://blogs.technet.com/b/privatecloud/archive/2012/03/19/cloud-datacenter-network-architecture-in-the-windows-server-8-era.aspx)<br />-Faça perguntas: [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-   VXLAN - [RFC 7348](https://www.rfc-editor.org/info/rfc7348)|  
|**Tecnologias relacionadas**|-   [Controlador de rede](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)<br />-   [Visão geral da virtualização de rede Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)|  
  


