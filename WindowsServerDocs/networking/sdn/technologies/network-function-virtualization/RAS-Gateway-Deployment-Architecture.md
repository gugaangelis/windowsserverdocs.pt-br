---
title: Arquitetura de implantação do Gateway de RAS
description: Você pode usar este tópico para saber mais sobre a implantação do CSP (provedor de serviços de nuvem) do gateway de RAS no Windows Server 2016, incluindo pools de gateway de RAS, refletores de rota e implantação de vários gateways para locatários individuais.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 91d8081261d3cbc5e2da61cc2b5a9737e76a0dc7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309799"
---
# <a name="ras-gateway-deployment-architecture"></a>Arquitetura de implantação do Gateway de RAS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre a implantação do CSP (provedor de serviços de nuvem) do gateway de RAS, incluindo pools de gateway de RAS, refletores de rota e implantação de vários gateways para locatários individuais.  
  
As seções a seguir fornecem breves visões gerais de alguns dos novos recursos do gateway de RAS para que você possa entender como usar esses recursos no design de sua implantação de gateway.  
  
Além disso, um exemplo de implantação é fornecido, incluindo informações sobre o processo de adição de novos locatários, sincronização de rotas e roteamento do plano de dados, failover do refletor de gateway e rota e muito mais.  
  
Este tópico contém as seguintes seções.  
  
-   [Usando novos recursos do gateway de RAS para projetar sua implantação](#bkmk_new)  
  
-   [Exemplo de implantação](#bkmk_example)  
  
-   [Adicionar novos locatários e espaço de CA (endereço do cliente) emparelhamento EBGP](#bkmk_tenant)  
  
-   [Sincronização de rotas e roteamento do plano de dados](#bkmk_route)  
  
-   [Como o controlador de rede responde ao gateway RAS e ao failover de refletor de rota](#bkmk_failover)  
  
-   [Vantagens de usar novos recursos de gateway de RAS](#bkmk_advantages)  
  
## <a name="using-ras-gateway-new-features-to-design-your--deployment"></a><a name="bkmk_new"></a>Usando novos recursos do gateway de RAS para projetar sua implantação  
O gateway de RAS inclui vários novos recursos que mudam e melhoram a maneira como você implanta sua infraestrutura de gateway em seu datacenter.  
  
### <a name="bgp-route-reflector"></a>Refletor de rota BGP  
A funcionalidade de refletor de rota Border Gateway Protocol (BGP) agora está incluída no gateway de RAS e fornece uma alternativa à topologia de malha completa do BGP que normalmente é necessária para a sincronização de rota entre roteadores. Com a sincronização de malha completa, todos os Roteadores BGP devem se conectar com todos os outros roteadores na topologia de roteamento. No entanto, quando você usa o refletor de rota, o refletor de rota é o único roteador que se conecta com todos os outros roteadores, chamados de clientes de refletor de rota BGP, simplificando assim a sincronização de rota e reduzindo o tráfego de rede. O refletor de rota aprende todas as rotas, calcula as melhores rotas e redistribui as melhores rotas para seus clientes BGP.  
  
Para obter mais informações, consulte [o que há de novo no gateway de Ras](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).  
  
### <a name="gateway-pools"></a><a name="bkmk_pools"></a>Pools de gateway  
No Windows Server 2016, você pode criar vários pools de gateway de tipos diferentes. Pools de gateway contêm muitas instâncias do gateway de RAS e roteiam o tráfego de rede entre redes físicas e virtuais.  
  
Para obter mais informações, consulte [novidades no gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [alta disponibilidade do gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="gateway-pool-scalability"></a><a name="bkmk_gps"></a>Escalabilidade do pool de gateway  
Você pode facilmente dimensionar um pool de gateway para cima ou para baixo adicionando ou removendo VMs de gateway no pool. A remoção ou a adição de gateways não interrompe os serviços fornecidos por um pool. Você também pode adicionar e remover pools inteiros de gateways.  
  
Para obter mais informações, consulte [novidades no gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [alta disponibilidade do gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="mn-gateway-pool-redundancy"></a><a name="bkmk_m"></a>M + N redundância de pool de gateway  
Cada pool de gateway é M + N redundante. Isso significa que o backup de um ' M' de VMs de gateway ativo é feito por um número ' N ' de VMs de gateway em espera. A redundância M + N fornece mais flexibilidade para determinar o nível de confiabilidade que você precisa ao implantar o gateway RAS.  
  
Para obter mais informações, consulte [novidades no gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [alta disponibilidade do gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="example-deployment"></a><a name="bkmk_example"></a>Exemplo de implantação  
A ilustração a seguir fornece um exemplo com emparelhamento eBGP em conexões VPN site a site configuradas entre dois locatários, contoso e Woodgrove, e o datacenter CSP da Fabrikam.  
  
![emparelhamento eBGP por VPN site a site](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
Neste exemplo, a contoso requer largura de banda de gateway adicional, levando à decisão de design de infraestrutura de gateway para encerrar o site contoso los Angeles em GW3 em vez de GW2. Por isso, as conexões VPN da Contoso de sites diferentes terminam no datacenter do CSP em dois gateways diferentes.  
  
Ambos os gateways, GW2 e GW3, foram os primeiros gateways de RAS configurados pelo controlador de rede quando o CSP adicionou os locatários contoso e Woodgrove à sua infraestrutura. Por isso, esses dois gateways são configurados como refletores de rota para esses clientes (ou locatários) correspondentes. GW2 é o refletor de rota da Contoso e GW3 é o refletor de rota do Woodgrove, além de ser o ponto de término do gateway de RAS do CSP para a conexão VPN com o site de HQ de los da Contoso Angeles.  
  
> [!NOTE]  
> Um gateway RAS pode rotear o tráfego de rede virtual e física para até 100 locatários diferentes, dependendo dos requisitos de largura de banda de cada locatário.  
  
Como os refletores de rota, o GW2 envia rotas de espaço de CA da Contoso para o controlador de rede e GW3 envia rotas de espaço da AC do Woodgrove para o controlador de rede.  
  
O controlador de rede envia políticas de virtualização de rede do Hyper-V para as redes virtuais contoso e Woodgrove, bem como políticas de RAS para os gateways RAS e políticas de balanceamento de carga para os multiplexadores (MUXes) configurados como um balanceamento de carga de software pool.  
  
## <a name="adding-new-tenants-and-customer-address-ca-space-ebgp-peering"></a><a name="bkmk_tenant"></a>Adicionar novos locatários e espaço de CA (endereço do cliente) emparelhamento eBGP  
Quando você assina um novo cliente e adiciona o cliente como um novo locatário em seu datacenter, você pode usar o processo a seguir, grande parte do qual é executado automaticamente pelo controlador de rede e pelos roteadores eBGP do gateway de RAS.  
  
1.  Provisione uma nova rede virtual e cargas de trabalho de acordo com os requisitos do seu locatário.  
  
2.  Se necessário, configure a conectividade remota entre o site corporativo de locatário remoto e sua rede virtual em seu datacenter. Quando você implanta uma conexão VPN site a site para o locatário, o controlador de rede seleciona automaticamente uma VM de gateway RAS disponível do pool de gateway disponível e configura a conexão.  
  
3.  Ao configurar a VM do gateway de RAS para o novo locatário, o controlador de rede também configura o gateway RAS como um roteador BGP e o designa como o refletor de rota para o locatário. Isso é verdadeiro mesmo em circunstâncias em que o gateway RAS serve como um gateway ou como um gateway e refletor de rota para outros locatários.  
  
4.  Dependendo se o roteamento de espaço da autoridade de certificação está configurado para usar redes configuradas estaticamente ou roteamento de BGP dinâmico, o controlador de rede configura as rotas estáticas correspondentes, vizinhos de BGP ou ambos na VM de gateway RAS e refletor de rota.  
  
    > [!NOTE]  
    > -   Depois que o controlador de rede tiver configurado um gateway RAS e um refletor de rota para o locatário, sempre que o mesmo locatário exigir uma nova conexão VPN site a site, o controlador de rede verificará a capacidade disponível nessa VM do gateway RAS. Se o gateway original puder atender à capacidade necessária, a nova conexão de rede também será configurada na mesma VM de gateway de RAS. Se a VM do gateway de RAS não puder lidar com capacidade adicional, o controlador de rede selecionará uma nova VM de gateway RAS disponível e configurará a nova conexão nela. Essa nova VM de gateway de RAS associada ao locatário se torna o cliente de refletor de rota do refletor de rota de gateway RAS de locatário original.  
    > -   Como os pools de gateway de RAS estão por trás dos balanceadores de carga de software (SLBs), os endereços VPN site a site dos locatários usam um único endereço IP público, chamado de VIP (endereço IP virtual), que é convertido pelo SLBs em um endereço IP interno de datacenter, chamado de Endereço IP dinâmico (DIP), para um gateway de RAS que roteia o tráfego para o locatário corporativo. Esse mapeamento de endereço IP público para privado por SLB garante que os túneis VPN site a site sejam corretamente estabelecidos entre os sites corporativos e os gateways RAS de CSP e os refletores de rota.  
    >   
    >     Para obter mais informações sobre SLB, VIPs e DIPs, consulte [ &#40;balanceamento de carga de&#41; software SLB para Sdn](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
5.  Depois que o túnel VPN site a site entre o site corporativo e o gateway de RAS do datacenter do CSP é estabelecido para o novo locatário, as rotas estáticas associadas aos túneis são automaticamente provisionadas nos lados Enterprise e CSP do túnel .  
  
6.  Com o roteamento de BGP de espaço de CA, o emparelhamento eBGP entre os sites corporativos e o refletor de rota de gateway RAS do CSP também é estabelecido.  
  
## <a name="route-synchronization-and-data-plane-routing"></a><a name="bkmk_route"></a>Sincronização de rotas e roteamento do plano de dados  
Depois que o emparelhamento eBGP é estabelecido entre os sites corporativos e o refletor de rota de gateway RAS do CSP, o refletor de rota aprende todas as rotas empresariais usando o roteamento de BGP dinâmico. O refletor de rota sincroniza essas rotas entre todos os clientes de refletor de rota para que eles sejam todos configurados com o mesmo conjunto de rotas.  
  
O refletor de rota também atualiza essas rotas consolidadas, usando a sincronização de rota para o controlador de rede. O controlador de rede converte as rotas nas políticas de virtualização de rede Hyper-V e configura a rede de malha para garantir que o roteamento de caminho de dados de ponta a ponta seja provisionado. Esse processo torna a rede virtual de locatário acessível a partir dos sites corporativos do locatário.  
  
Para o roteamento do plano de dados, os pacotes que atingem as VMs do gateway RAS são roteados diretamente para a rede virtual do locatário, pois as rotas necessárias agora estão disponíveis com todas as VMs de gateway de RAS participantes.  
  
Da mesma forma, com as políticas de virtualização de rede do Hyper-V em vigor, a rede virtual de locatário roteia os pacotes diretamente para as VMs do gateway de RAS (sem a necessidade de saber mais sobre o refletor de rota) e, em seguida, para os sites corporativos nos túneis VPN site a site .  
  
Além disso. o tráfego de retorno da rede virtual de locatário para o site corporativo de locatário remoto ignora o SLBs, um processo chamado DSR (retorno de servidor direto).  
  
## <a name="how-network-controller-responds-to-ras-gateway-and-route-reflector-failover"></a><a name="bkmk_failover"></a>Como o controlador de rede responde ao gateway RAS e ao failover de refletor de rota  
A seguir estão dois cenários de failover possíveis: um para clientes de refletor de rota de gateway de RAS e outro para os refletores de rota de gateway de RAS-incluindo informações sobre como o controlador de rede lida com failover para VMs em qualquer configuração.  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>Falha de VM de um cliente de refletor de rota BGP de gateway de RAS  
O controlador de rede executa as seguintes ações quando um cliente do refletor de rota de gateway RAS falha.  
  
> [!NOTE]  
> Quando um gateway de RAS não é um refletor de rota para a infraestrutura BGP de um locatário, ele é um cliente de refletor de rota na infraestrutura BGP do locatário.  
  
-   O controlador de rede seleciona uma VM de gateway de RAS em espera disponível e provisiona a nova VM de gateway de RAS com a configuração da VM de gateway de RAS com falha.  
  
-   O controlador de rede atualiza a configuração de SLB correspondente para garantir que os túneis de VPN site a site de sites de locatário para o gateway de RAS com falha sejam estabelecidos corretamente com o novo gateway de RAS.  
  
-   O controlador de rede configura o cliente de refletor de rota BGP no novo gateway.  
  
-   O controlador de rede configura o novo cliente de refletor de rota BGP de gateway de RAS como ativo. O gateway RAS inicia imediatamente o emparelhamento com o refletor de rota do locatário para compartilhar informações de roteamento e habilitar o emparelhamento eBGP para o site corporativo correspondente.  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>Falha de VM para um refletor de rota BGP de gateway de RAS  
O controlador de rede executa as seguintes ações quando um refletor de rota BGP de gateway RAS falha.  
  
-   O controlador de rede seleciona uma VM de gateway de RAS em espera disponível e provisiona a nova VM de gateway de RAS com a configuração da VM de gateway de RAS com falha.  
  
-   O controlador de rede configura o refletor de rota na nova VM de gateway de RAS e atribui a nova VM o mesmo endereço IP que foi usado pela VM com falha, fornecendo, assim, a integridade da rota, apesar da falha da VM.  
  
-   O controlador de rede atualiza a configuração de SLB correspondente para garantir que os túneis de VPN site a site de sites de locatário para o gateway de RAS com falha sejam estabelecidos corretamente com o novo gateway de RAS.  
  
-   O controlador de rede configura a nova VM de refletor de rota BGP de gateway de RAS como ativa.  
  
-   O refletor de rota fica imediatamente ativo. O túnel VPN site a site para a empresa é estabelecido e o refletor de rota usa as rotas de emparelhamento e trocas eBGP com os roteadores de site corporativo.  
  
-   Após a seleção de rota de BGP, o refletor de rota BGP de gateway de RAS atualiza os clientes de refletor de rota de locatário no datacenter e sincroniza as rotas com o controlador de rede, disponibilizando o caminho de dados de ponta a ponta para o tráfego de locatário.  
  
## <a name="advantages-of-using-new-ras-gateway-features"></a><a name="bkmk_advantages"></a>Vantagens de usar novos recursos de gateway de RAS  
A seguir estão algumas das vantagens de usar esses novos recursos de gateway de RAS ao projetar sua implantação de gateway de RAS.  
  
**Escalabilidade do gateway de RAS**  
  
Como você pode adicionar tantas VMs de gateway de RAS quanto precisar para pools de gateway de RAS, você pode dimensionar facilmente sua implantação de gateway de RAS para otimizar o desempenho e a capacidade. Ao adicionar VMs a um pool, você pode configurar esses gateways RAS com conexões VPN site a site de qualquer tipo (IKEv2, L3, GRE), eliminando afunilamentos de capacidade sem tempo de inatividade.  
  
**Gerenciamento simplificado do gateway de site corporativo**  
  
Quando seu locatário tem vários sites corporativos, o locatário pode configurar todos os sites com um endereço IP de VPN site a site remoto e um único endereço IP vizinho remoto-seu CSP de rota BGP de gateway de RAS do datacenter para esse locatário. Isso simplifica o gerenciamento de gateway para seus locatários.  
  
**Correção rápida da falha do gateway**  
  
Para garantir uma resposta rápida de failover, você pode configurar o tempo de parâmetro BGP KeepAlive entre rotas de borda e o roteador de controle em um intervalo de tempo curto, como menor ou igual a dez segundos. Com esse intervalo curto de Keep Alive, se um roteador de borda BGP de gateway RAS falhar, a falha será detectada rapidamente e o controlador de rede seguirá as etapas fornecidas nas seções anteriores. Essa vantagem pode reduzir a necessidade de um protocolo de detecção de falha separado, como o protocolo BFD (detecção de encaminhamento bidirecional).  
  
 
  


