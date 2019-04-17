---
title: Visão geral de virtualização de rede do Hyper-V no Windows Server 2016
description: Este tópico fornece uma visão geral de virtualização de rede do Hyper-V no Windows Server 2016
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
ms.openlocfilehash: c7c20f9b2d81ac2d49ed0bbea5f3aca48dea0c50
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="hyper-v-network-virtualization-overview-in-windows-server-2016"></a>Visão geral de virtualização de rede do Hyper-V no Windows Server 2016

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

No Windows Server 2016 e Gerenciador de máquina Virtual, a Microsoft fornece uma solução de virtualização de rede de ponta a ponta.  Há cinco componentes principais que compõem a solução de virtualização de rede da Microsoft:  
  
-   **Windows Azure Pack para Windows Server** fornece um locatário voltado para o portal para criar redes virtuais e um portal administrativo para gerenciar redes virtuais.  
  
-   **Gerenciador de máquina virtual** (VMM) fornece gerenciamento centralizado da malha de rede.  
  
-   **Controlador de rede Microsoft** fornece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede físicos e virtuais no seu datacenter.  
  
-   **Virtualização de rede do Hyper-V** fornece a infraestrutura necessária para virtualizar o tráfego de rede.  
  
-   **Gateways de virtualização de rede do Hyper-V** fornecem conexões entre redes físicos e virtuais.  
  
Este tópico apresenta os conceitos e explica os principais benefícios e as funcionalidades de virtualização de rede do Hyper-V (uma parte da solução de virtualização de rede geral) no Windows Server 2016. Ela explica como ambas as nuvens privadas procurando consolidação de carga de trabalho corporativa e provedores de serviços de nuvem pública da infraestrutura como serviço (IaaS) de benefícios de virtualização de rede.  
  
Para obter mais detalhes técnicos sobre a virtualização de rede no Windows Server 2016, consulte [detalhes técnicos do Hyper-V rede virtualização no Windows Server 2016](../../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md).  
  
**Você quis dizer**  
  
-   [Visão geral de virtualização do Hyper-V rede](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)  
  
-   [Visão geral do Hyper-V](assetId:///5aad349f-ef06-464a-b36f-366fbb040143)  
  
-   [Visão geral do Hyper-V Virtual Switch](assetId:///e6ec46af-6ef4-49b3-b1f1-5268dc03f05b)  
  
## <a name="BKMK_OVER"></a>Descrição dos recursos  
Virtualização de rede do Hyper-V oferece "redes virtuais" (chamados de uma rede VM) para máquinas virtuais semelhantes a como virtualização de servidor (hipervisor) fornece "máquinas virtuais" para o sistema operacional. Virtualização de rede separa os redes virtuais a partir da infraestrutura de rede física e remove as restrições de VLAN e hierárquica atribuição de endereço IP da máquina virtual de provisionamento. Essa flexibilidade torna fácil para os clientes mover a IaaS nuvens e eficientes para hosters e datacenter administradores para gerenciar sua infraestrutura, mantendo o isolamento de vários locatário necessários, requisitos de segurança, e endereços IP de máquina Virtual sobrepostas suporte.  
  
Os clientes querem perfeitamente estender seus centros de dados na nuvem. Atualmente, há desafios técnicos tomar tais arquiteturas de nuvem híbrida perfeita. Um da face maior de clientes de obstáculos é reutilizar seus topologias existentes de rede (sub-redes, IP endereços, serviços de rede e assim por diante) na nuvem e ponte entre seus recursos locais e seus recursos de nuvem.  Virtualização de rede do Hyper-V oferece o conceito de uma rede de VM é independente da rede física subjacente. Com esse conceito de uma rede de VM, composta de uma ou mais sub-redes virtuais, o local exato na rede física de máquinas virtuais conectados a uma rede virtual é dissociado a topologia de rede virtual. Como resultado, os clientes podem facilmente mover suas sub-redes virtuais na nuvem, preservando os endereços IP existentes e a topologia na nuvem para que os serviços existentes continuam a funcionar sem reconhecimento da localização física das sub-redes. Ou seja, a virtualização de rede do Hyper-V permite que uma nuvem híbrida perfeita.  
  
Além de nuvem híbrida, muitas organizações são consolidando seus centros de dados e criação de nuvens privadas para internamente obtenham o benefício de eficiência e escalabilidade de arquiteturas de nuvem. Virtualização de rede do Hyper-V permite melhor flexibilidade e a eficiência de nuvens privadas por dissociação uma divisão topologia de rede (fazendo com que ele virtual) de topologia de rede física real. Dessa forma, as unidades de negócios podem facilmente compartilhe uma nuvem privada interno enquanto está sendo isolados uns dos outros e continuar manter topologias de rede existente. A equipe de operações de datacenter tem flexibilidade para implantar e mover dinamicamente cargas de trabalho em qualquer lugar em um datacenter mais eficaz geral e o datacenter sem interrupções de servidor fornecer melhor eficiência operacional.  
  
Para proprietários de carga de trabalho, a principal vantagem é que eles agora podem mover sua carga de trabalho "topologias" na nuvem sem alterar seus endereços IP ou escrever novamente seus aplicativos. Por exemplo, o aplicativo de LOB três camadas típico é composto de uma faixa de front-end, uma camada de lógica de negócios e uma faixa de banco de dados. Por meio da política, virtualização de rede do Hyper-V permite que cliente integração todos ou partes dos três níveis na nuvem, mantendo a topologia de roteamento e os endereços IP dos serviços (ou seja, endereços IP máquina virtual), sem a necessidade dos aplicativos sejam alteradas.  
  
Para proprietários de infraestrutura, a flexibilidade adicional na colocação de máquina virtual torna possível mover as cargas de trabalho os datacenters em qualquer lugar sem alterar as máquinas virtuais ou reconfigurar as redes. Por exemplo virtualização de rede do Hyper-V permite a migração dinâmica de sub-rede cruzado para que uma máquina virtual pode migrar ao vivo em qualquer lugar no datacenter sem uma interrupção do serviço. Anteriormente, migração em tempo real foi limitada à mesma sub-rede restringindo onde máquinas virtuais pode ser localizadas. Cruzada sub-rede migração ao vivo permite que os administradores consolidar cargas de trabalho com base nos requisitos de recursos dinâmicos, eficiência de energia e também pode acomodar a manutenção de infraestrutura sem interromper a carga de trabalho do cliente o tempo.  
  
## <a name="BKMK_APP"></a>Aplicativos práticos  
Com o sucesso de datacenters virtualizados, provedores de hospedagem (provedores que oferecem a colocação ou aluguéis de servidor físico) e as organizações de TI começaram a oferecer mais flexíveis infraestruturas virtualizadas que facilitam a oferecer instâncias do servidor sob demanda para seus clientes. Essa nova classe de serviço é conhecido como infraestrutura como serviço (IaaS). Windows Server 2016 fornece todos os recursos de plataforma necessária para permitir que os clientes corporativos a criação de nuvens privadas e transição para um IT como um modelo de serviço operacionais. Windows Server 2016 2016 também permite hosters criar nuvens públicas e oferecer soluções IaaS aos seus clientes. Quando combinado com Virtual Machine Manager e o Windows Azure Pack para gerenciar a diretiva de virtualização de rede do Hyper-V, a Microsoft fornece uma solução de nuvem potente.  
  
Virtualização de rede do Windows Server 2016 Hyper-V fornece virtualização de software controlado, com base em política de rede que reduz o sobrecarga enfrentado por empresas quando eles serão expandidos dedicados nuvens de IaaS, e fornece hosters de nuvem melhor flexibilidade e escalabilidade para o gerenciamento de máquinas virtuais para alcançar mais alta utilização de recursos de gerenciamento.  
  
Um cenário de IaaS que tenha máquinas virtuais de diferentes divisões organizacionais (nuvem dedicados) ou clientes diferentes (nuvem hospedado) requer isolamento seguro. Solução de hoje, redes locais virtuais (VLANs), pode apresentar desvantagens significativas neste cenário.  
  
**VLANs**  
  
Atualmente, VLANs são os mecanismos que a maioria das organizações usam para dar suporte a reutilização de espaço de endereço e isolamento do locatário. Uma VLAN usa explícito marcação (VLAN ID) nos cabeçalhos de quadros Ethernet, e ele depende de switches de Ethernet para impor o isolamento e restringir o tráfego para nós de rede com a mesma ID de VLAN. As desvantagens principais com VLANs são as seguintes:  
  
-   Um risco maior de uma interrupção inadvertida devido a reconfiguração complicada de produção alterna sempre que mover máquinas virtuais ou limites de isolamento no datacenter dinâmico.  
  
-   Limitado de escalabilidade porque há um máximo de 4094 VLANs e comutadores típicos suportam não mais de 1000 IDs de VLAN.  
  
-   Restrito dentro de uma única sub-rede IP, que limita o número de nós dentro de uma única VLAN e restringe o posicionamento de máquinas virtuais com base em locais físicos. Embora VLANs podem ser expandidos em sites, toda a VLAN deve estar na mesma sub-rede.  
  
**Atribuição de endereço IP**  
  
As desvantagens são apresentadas pelo VLANs, além de atribuição de endereço IP de máquina virtual apresenta problemas, que incluem:  
  
-   Locais físicos na infraestrutura de rede datacenter determinam endereços IP de máquina virtual. Como resultado, migrando para a nuvem normalmente requer a alteração de endereços IP das cargas de trabalho do serviço.  
  
-   As políticas são vinculadas aos endereços IP, como as regras de firewall, serviços de descoberta e o diretório de recursos e assim por diante. Alterar endereços IP requer a atualização todas as diretivas associadas.  
  
-   Implantação de máquinas virtuais e isolamento de tráfego dependem a topologia.  
  
Quando os administradores de rede datacenter planejar o layout físico do data center, eles devem tomar decisões sobre onde sub-redes serão fisicamente colocados e roteados. Essas decisões são baseadas em tecnologia IP e Ethernet que influenciam os potenciais endereços IP que são permitidos para máquinas virtuais em execução em um determinado servidor ou uma lâmina está conectada a um determinado rack no datacenter. Quando uma máquina virtual é provisionada e colocada no datacenter, devem cumprir essas escolhas e restrições sobre o endereço IP. Portanto, o resultado mais comum é que os administradores de datacenter atribuem novos endereços IP para máquinas virtuais.  
  
O problema com esse requisito é que, além de ser um endereço, há semânticas informações associadas a um endereço IP. Por exemplo, uma sub-rede pode conter fornecido serviços ou estar em um local físico distinto. As regras de firewall, políticas de controle de acesso e associações de segurança IPsec são frequentemente associadas a endereços IP. Alterar endereços IP força os proprietários de máquina virtual para ajustar todas as suas políticas foram com base no endereço IP original. Este renumeração sobrecarga é tão alto que muitas empresas optou por implantar apenas novos serviços na nuvem, deixando somente aplicativos herdados.  
  
Virtualização de rede do Hyper-V separa os redes virtuais para máquinas virtuais cliente a partir da infraestrutura de rede física. Como resultado, ela permite que máquinas virtuais do cliente manter seus endereços IP originais, permitindo que os administradores de datacenter provisionar máquinas virtuais de cliente em qualquer lugar no datacenter sem reconfiguração físico endereços IP ou IDs de VLAN. A próxima seção resume a funcionalidade de teclas.  
  
## <a name="BKMK_NEW"></a>Funcionalidade importante  
A seguir está uma lista dos principais funcionalidades, benefícios e recursos de virtualização de rede do Hyper-V no Windows Server 2016:  
  
-   **Permite que o posicionamento de carga de trabalho flexível - isolamento de rede e endereço IP novamente usar sem VLANs**  
  
    Virtualização de rede do Hyper-V separa os redes virtuais do cliente da infraestrutura de rede física de hosters, fornecendo liberdade para posicionamentos de carga de trabalho dentro de data centers. Posicionamento da carga de trabalho de máquina virtual não é limitado a atribuição de endereço IP ou requisitos de isolamento de VLAN da rede física porque ela é imposta em hosts Hyper-V com base em virtualização e definidos pelo software, vários locatários políticas.  
  
    Máquinas virtuais de diferentes clientes com endereços IP sobrepostos agora pode ser implantadas no mesmo servidor host sem exigir configuração VLAN complicada ou violar a hierarquia de endereço IP. Isso pode simplificar a migração de cargas de trabalho do cliente em compartilhado IaaS hospedagem provedores, permitindo que os clientes mover essas cargas de trabalho sem modificação, que inclui deixar os endereços IP de máquina virtual inalterados. Para o provedor de hospedagem, o suporte a vários clientes que desejam estender seu espaço de endereço de rede existente para o datacenter IaaS compartilhado é um exercício complexo de configurar e manter VLANs isolados para cada cliente garantir a coexistência de potencialmente sobrepostos espaços de endereço. Com a virtualização de rede do Hyper-V, dando suporte a endereços sobrepostos fica mais fácil e exige menos reconfiguração de rede pelo provedor de hospedagem.  
  
    Além disso, as atualizações e manutenção de infraestrutura físico podem ser feitas sem causar um tempo de inatividade de cargas de trabalho do cliente. Com a virtualização de rede do Hyper-V, máquinas virtuais em um host específico, rack, sub-rede, VLAN ou todo cluster podem ser migradas sem exigir uma alteração de endereço IP física ou reconfiguração principais.  
  
-   **Permite que move mais fácil para cargas de trabalho para uma nuvem de IaaS compartilhada**  
  
    Com a virtualização de rede do Hyper-V, endereços IP e a máquina virtual configurações permanecerão inalteradas. Isso permite que as organizações de TI mover cargas de trabalho mais facilmente de seus centros de dados para um provedor de hospedagem de IaaS compartilhado com o mínimo reconfiguração da carga de trabalho ou suas ferramentas de infraestrutura e políticas. Em casos onde há conectividade entre dois centros de dados, os administradores de TI podem continuar a usar suas ferramentas sem reconfigurá-los.  
  
-   **Habilita a migração ao vivo em sub-redes**  
  
    Migração ao vivo das cargas de trabalho de máquina virtual tradicionalmente foi limitada à mesma sub-rede IP ou VLAN porque cruzar sub-redes necessária sistema operacional a máquina virtual para alterar seu endereço IP. Essa alteração de endereço quebras de comunicação existente e interrompe os serviços em execução na máquina virtual. Com a virtualização de rede do Hyper-V, as cargas de trabalho podem ser dinâmicas migrados de servidores que executam o Windows Server 2016 em uma sub-rede com servidores que executam o Windows Server 2016 em uma sub-rede diferente sem alterar os endereços IP de carga de trabalho. Virtualização do Hyper-V rede garante que as alterações de local de máquina virtual devido a migração ao vivo são atualizadas e sincronizadas entre os hosts que têm contínua comunicação com a máquina virtual migrada.  
  
-   **Permite o gerenciamento mais fácil de administração de servidor e rede desacoplada**  
  
    Posicionamento de carga de trabalho do servidor é simples porque a migração e o posicionamento das cargas de trabalho são independentes das configurações de rede física subjacente. Os administradores de servidor podem se concentrar no gerenciamento de serviços e servidores, e os administradores de rede podem se concentrar no gerenciamento de infraestrutura e tráfego de rede geral. Isso permite que os administradores de servidor de datacenter implantar e migrar máquinas virtuais sem alterar os endereços IP das máquinas virtuais. Lá é reduzida sobrecarga como virtualização de rede do Hyper-V permite que o posicionamento de máquina virtual para ocorrer independentemente da topologia de rede, reduzindo a necessidade dos administradores de rede para se envolver com posicionamentos que podem alterar os limites de isolamento.  
  
-   **Simplifica a rede e melhora a utilização de recursos de rede/server**  
  
    Rigidez de VLANs e a dependência de posicionamento de máquina virtual em uma infraestrutura de rede física resultam em excesso de provisionamento e subutilização. Ao quebrar a dependência, a maior flexibilidade de posicionamento de carga de trabalho de máquina virtual pode simplificar o gerenciamento de rede e melhorar o servidor e utilização de recursos de rede. Observe que o Hyper-V virtualização de rede compatível com VLANs no contexto de datacenter físico. Por exemplo, um data center pode desejar todo o tráfego de virtualização de rede do Hyper-V para ser em um VLAN específico.  
  
-   **É compatível com a infraestrutura existente e tecnologia emergente**  
  
    Virtualização de rede do Hyper-V pode ser implantada no datacenter de hoje, mas ele é compatível com tecnologias de "rede simples" datacenter emergentes.  
  
    Por exemplo, HNV no Windows Server 2016 suporta o formato de encapsulamento VXLAN e abrir vSwitch protocolo de gerenciamento de banco de dados (OVSDB) como a Interface SouthBound (SBI)...  
  
-   **Fornece para preparação de interoperabilidade e ecossistema**  
  
    Virtualização do Hyper-V rede dá suporte a várias configurações para comunicação com recursos existentes, como cruzada conectividade local, rede de área de armazenamento (SAN), acesso a recursos não virtualizados e assim por diante. A Microsoft está comprometida a trabalhar com parceiros do ecossistema suporte e aprimorar a experiência de virtualização de rede do Hyper-V em termos de capacidade de gerenciamento, desempenho e escalabilidade.  
  
-   **Com base em política de configuração**  
  
    Políticas de virtualização de rede no Windows Server 2016 são configuradas por meio do controlador de rede Microsoft. O controlador de rede tem uma API em sentido norte RESTful e a interface do Windows PowerShell para configurar a política. Para saber mais sobre o controlador de rede Microsoft, consulte [rede controlador](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md).  
  
## <a name="BKMK_SOFT"></a>Requisitos de software  
Virtualização de rede do Hyper-V usando o controlador de rede Microsoft requer o Windows Server 2016 e a função Hyper-V.  
  
## <a name="BKMK_LINKS"></a>Consulte também  
Para saber mais sobre a virtualização de rede do Hyper-V no Windows Server 2016, consulte os links a seguir:  
  
|Tipo de conteúdo|Referências|  
|----------------|--------------|  
|**Recursos da comunidade**|-   [Blog da arquitetura de nuvem privada](http://blogs.technet.com/b/privatecloud/archive/2012/03/19/cloud-datacenter-network-architecture-in-the-windows-server-8-era.aspx)<br />-Perguntas:[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-VXLAN - [RFC 7348](http://www.rfc-editor.org/info/rfc7348)|  
|**Tecnologias relacionadas**|-   [Controlador de rede](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)<br />-   [Visão geral de virtualização do Hyper-V rede](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)|  
  


