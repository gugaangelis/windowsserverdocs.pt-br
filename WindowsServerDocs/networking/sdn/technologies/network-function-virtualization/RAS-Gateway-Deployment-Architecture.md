---
title: Arquitetura de implantação do RAS Gateway
description: Você pode usar este tópico para saber mais sobre a implantação de provedor de serviços de nuvem (CSP) do Gateway RAS em Windows Server 2016, incluindo pools de Gateway RAS, Refletores de rota e implantar vários gateways para locatários individuais.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 21b101e10dba3d3b9578d6804b4fd92fbbcd2167
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-deployment-architecture"></a>Arquitetura de implantação do RAS Gateway

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre a implantação de provedor de serviços de nuvem (CSP) do Gateway RAS, incluindo pools de Gateway RAS, Refletores de rota e implantar vários gateways para locatários individuais.  
  
As seguintes seções fornecem breves visões gerais de alguns dos novos recursos de Gateway RAS para que você possa entender como usar esses recursos no design da implantação do gateway.  
  
Além disso, uma implantação de exemplo é fornecida, incluindo informações sobre o processo de adição de locatários novo, sincronização de rota e roteamento de plano de dados, gateway e failover refletor de rota e muito mais.  
  
Este tópico contém as seções a seguir.  
  
-   [Usando o Gateway RAS novos recursos para projetar sua implantação](#bkmk_new)  
  
-   [Exemplo de implantação](#bkmk_example)  
  
-   [Adicionando novos locatários e espaço de endereço (CA) do cliente EBGP estranhas](#bkmk_tenant)  
  
-   [Roteamento de plano de dados e sincronização de rota](#bkmk_route)  
  
-   [Como o controlador de rede responde a Gateway RAS e rota Reflector Failover](#bkmk_failover)  
  
-   [Vantagens de usar os novos recursos de Gateway RAS](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>Usando o Gateway RAS novos recursos para projetar sua implantação  
Gateway RAS inclui vários novos recursos que mudam e melhoram a forma em que você implantar sua infraestrutura de gateway em seu datacenter.  
  
### <a name="bgp-route-reflector"></a>Rota BGP Reflector  
A funcionalidade de refletor de rota Border Gateway Protocol (BGP) agora é incluído com o Gateway RAS e oferece uma alternativa a topologia em malha completa BGP que normalmente é necessária para a sincronização de rota entre roteadores. Com a sincronização de malha completo, todos os roteadores BGP devem se conectar com todos os outros roteadores a topologia de roteamento. Quando você usa refletor de rota, no entanto, o refletor de rota é o único roteador que se conecta com todos os outros roteadores, chamados refletor de rota BGP clientes, simplificando, assim, a sincronização de rota e reduzir o tráfego de rede. O refletor de rota aprende sobre todas as rotas, calcula melhores rotas e redistribui as rotas melhor a seus clientes BGP.  
  
Para obter mais informações, consulte [What's New em Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).  
  
### <a name="bkmk_pools"></a>Pools de gateway  
No Windows Server 2016, você pode criar grupos de gateway muitos dos tipos diferentes. Pools de gateway contêm muitas instâncias do Gateway RAS e rotear o tráfego de rede entre redes físicos e virtuais.  
  
Para obter mais informações, consulte [What's New em Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [RAS Gateway alta disponibilidade](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_gps"></a>Dimensionamento do Pool de gateway  
Você pode dimensionar facilmente um pool de gateway para cima ou para baixo, adicionando ou removendo gateway VMs no pool. Remoção ou a adição de gateways não interromper os serviços que são fornecidos por um pool. Você também pode adicionar e remover todos pools de gateways.  
  
Para obter mais informações, consulte [What's New em Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [RAS Gateway alta disponibilidade](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_m"></a>M + N Gateway Pool redundância  
Cada pool de gateway é M + N redundante. Isso significa que um estou ' diversas ativa gateway VMs são copiadas por um número de'n' de gateway espera VMs. M + N redundância fornece mais flexibilidade para determinar o nível de confiabilidade que exigem quando você implanta o Gateway RAS.  
  
Para obter mais informações, consulte [What's New em Gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) e [RAS Gateway alta disponibilidade](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_example"></a>Exemplo de implantação  
A ilustração a seguir fornece um exemplo com correspondências em conexões de VPN-to-site configuradas entre dois locatários, Contoso e Woodgrove e datacenter Fabrikam CSP eBGP.  
  
![Correspondências pela VPN to-site eBGP](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
Neste exemplo, Contoso requer largura de banda de gateway adicional, provocando a decisão de design de infraestrutura de gateway de encerrar o site Contoso Los Angeles GW3 em vez de GW2. Por isso, conexões VPN Contoso de sites diferentes encerrar no datacenter CSP em dois gateways diferentes.  
  
Ambas essas gateways, GW2 e GW3, foram os primeiros Gateways de RAS configurado pelo controlador de rede quando o CSP adicionado as Contoso e Woodgrove locatários à sua infraestrutura. Por isso, esses dois gateways são configurados como Refletores de rota para esses clientes correspondentes (ou locatários). GW2 é o refletor de rota Contoso e GW3 é o refletor de rota Woodgrove - além de ser o ponto de término do CSP RAS Gateway para a conexão VPN com o site HQ de Los Angeles Contoso.  
  
> [!NOTE]  
> Um Gateway RAS pode encaminhar o tráfego de rede físicos e virtuais para locatários diferentes até cem, dependendo dos requisitos de largura de banda de cada locatário.  
  
Como Refletores de rota, GW2 envia rotas Contoso CA espaço para o controlador de rede e GW3 envia rotas Woodgrove CA espaço para o controlador de rede.  
  
Controlador de rede empurra políticas de virtualização de rede do Hyper-V para a Contoso e Woodgrove redes virtuais, bem como as políticas RAS à Gateways RAS e políticas para Multiplexers (MUXes) que estão configurados como um pool de balanceamento de carga de Software de balanceamento de carga.  
  
## <a name="bkmk_tenant"></a>Adicionando eBGP locatários novos e espaço de endereço do cliente (CA) Peering  
Quando você entra um novo cliente e adicionar o cliente como um locatário novo em seu datacenter, você pode usar o processo a seguir, grande parte delas é realizado automaticamente pelo controlador de rede e Gateway RAS roteadores eBGP.  
  
1.  Provisione uma nova rede virtual e cargas de trabalho de acordo com os requisitos do locatário.  
  
2.  Se necessário, configure a conectividade remota entre o site de Enterprise locatário remoto e sua rede virtual no seu datacenter. Quando você implanta uma conexão de VPN-to-site do locatário, o controlador de rede automaticamente seleciona uma VM de Gateway RAS disponível do pool gateway disponíveis e configura a conexão.  
  
3.  Ao configurar a VM do Gateway RAS do locatário novo, o controlador de rede também configura o Gateway RAS como um roteador BGP e designa como o refletor de rota do locatário. Isso vale mesmo em casos onde o Gateway RAS serve como um gateway, ou como um gateway e refletor de rota para outros locatários.  
  
4.  Dependendo se CA espaço roteamento é configurado para usar configurada estaticamente redes ou roteamento de BGP dinâmico, o controlador de rede configura as rotas estáticas correspondentes, vizinhos de BGP ou ambos nos RAS Gateway VM e refletor de rota.  
  
    > [!NOTE]  
    > -   Depois que o controlador de rede tiver configurado um Gateway RAS e refletor de rota de locatário, sempre que o mesmo locatário requer uma nova conexão de VPN-to-site, o controlador de rede verifica a capacidade disponível nessa VM de Gateway RAS. Se o gateway original pode atender a capacidade necessária, a nova conexão de rede também é configurado na mesma VM Gateway RAS. Se a VM do Gateway RAS não pode lidar com a capacidade adicional, o controlador de rede seleciona uma nova VM de Gateway RAS disponíveis e configura a nova conexão nele. Essa nova VM de Gateway RAS associados com o locatário torna-se o cliente de refletor de rota do locatário original refletor de rota de Gateway RAS.  
    > -   Como pools de Gateway RAS são atrás balanceadores de carga de Software (SLBs),-to-site VPN o locatários aborda cada uso um único endereço IP público, chamado de um endereço IP virtual (VIP), que é traduzido pelos SLBs em um endereço IP datacenter interno, chamado de um endereço IP dinâmico DIP (), um gateway RAS que encaminha o tráfego do locatário Enterprise. Esse mapeamento de endereço IP público para privado por SLB garante que o túneis VPN-to-site são estabelecidos corretamente entre os sites da empresa e a CSP RAS Gateways e Refletores de rota.  
    >   
    >     Para obter mais informações sobre SLB, VIPs e DIPs, consulte [balanceamento de carga de Software & #40; SLB & #41; para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
5.  Após o encapsulamento VPN-to-site entre o site da empresa e datacenter CSP que gateway RAS é estabelecida para o novo locatário, as rotas estáticas que são associadas as túneis automaticamente provisionadas nos lados do Enterprise e o CSP do encapsulamento.  
  
6.  Com o espaço de CA BGP roteamento, a correspondências entre os sites corporativos e a CSP RAS Gateway rota Reflector eBGP também é estabelecida.  
  
## <a name="bkmk_route"></a>Roteamento de plano de dados e sincronização de rota  
Depois de correspondência eBGP é estabelecida entre sites corporativos e a CSP RAS Gateway rota Reflector, o refletor de rota aprende todas as rotas de empresa usando o roteamento de BGP dinâmico. O refletor de rota sincroniza essas rotas entre todos os clientes refletor de rota para que eles são configurados com o mesmo conjunto de rotas.  
  
Refletor de rota também atualiza essas rotas consolidadas, usando a sincronização de rota, controlador de rede. Controlador de rede, em seguida, converte as rotas em políticas de virtualização de rede do Hyper-V e configura a rede de malha para garantir que End-to-End roteamento de caminho de dados é provisionado. Esse processo faz com que o locatário rede virtual acessível a partir do locatário Enterprise sites.  
  
Roteamento de plano de dados, os pacotes que chegar VMs do Gateway RAS diretamente são roteados para rede virtual do locatário, porque as rotas necessárias agora estão disponíveis com todas as VMs do Gateway RAS participantes.  
  
Da mesma forma, com as políticas de virtualização de rede do Hyper-V no lugar, a rede virtual locatário roteia pacotes diretamente para VMs do Gateway RAS (sem a necessidade de saber sobre o refletor de rota) e, em seguida, os sites corporativos sobre os túneis VPN-to-site.  
  
Além disso. Retorno tráfego da rede virtual locatário no site do Enterprise locatário remoto ignora SLBs, um processo chamado retornar de servidor direto (DSR).  
  
## <a name="bkmk_failover"></a>Como o controlador de rede responde a Gateway RAS e rota Reflector Failover  
A seguir é dois cenários possíveis de failover - uma para clientes de refletor de rota de Gateway RAS - e outra para Refletores de rota de Gateway RAS incluindo informações sobre como o controlador de rede manipula failover para VMs em configuração.  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>VM falha de um cliente do RAS Gateway rota BGP Reflector  
Controlador de rede tem as seguintes ações quando um cliente RAS refletor de rota de Gateway falha.  
  
> [!NOTE]  
> Quando um Gateway RAS não é um refletor de rota de infraestrutura BGP do locatário, ele é um cliente de refletor de rota na infraestrutura BGP do locatário.  
  
-   Controlador de rede seleciona uma VM de Gateway RAS espera disponíveis e provisiona a VM de Gateway RAS novo com a configuração do RAS Gateway VM com falha.  
  
-   Controlador de rede atualiza a configuração de SLB correspondente para garantir que o túneis VPN-to-site de sites de locatário para o Gateway RAS com falha são estabelecidos corretamente com o novo Gateway RAS.  
  
-   Controlador de rede configura o cliente de refletor de rota BGP no novo gateway.  
  
-   Controlador de rede configura o novo cliente RAS Gateway BGP rota Reflector como ativa. O Gateway RAS começa imediatamente a correspondência com refletor do locatário de rota para compartilhar informações de roteamento e habilitar a correspondência eBGP para o site Enterprise correspondente.  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>VM falha para uma rota BGP o Gateway RAS Reflector  
Controlador de rede tem as seguintes ações quando um refletor de rota RAS Gateway BGP falha.  
  
-   Controlador de rede seleciona uma VM de Gateway RAS espera disponíveis e provisiona a VM de Gateway RAS novo com a configuração do RAS Gateway VM com falha.  
  
-   Controlador de rede configura o refletor de rota na nova VM Gateway RAS e atribui a VM novo o mesmo endereço IP que foi usado pela VM com falha, oferecendo integridade rota apesar da falha VM.  
  
-   Controlador de rede atualiza a configuração de SLB correspondente para garantir que o túneis VPN-to-site de sites de locatário para o Gateway RAS com falha são estabelecidos corretamente com o novo Gateway RAS.  
  
-   Controlador de rede configura novos RAS Gateway BGP rota Reflector VM como ativa.  
  
-   O refletor de rota imediatamente se torna ativo. O encapsulamento VPN-to-site para a empresa é estabelecido e o refletor de rota usa correspondência eBGP e troca rotas com roteadores Enterprise site.  
  
-   Após a seleção de rota BGP, as atualizações de refletor de rota RAS Gateway BGP clientes refletor de rota no datacenter do locatário e sincroniza rotas com o controlador de rede, tornando o caminho dos dados End-to-End disponíveis para o tráfego de locatário.  
  
## <a name="bkmk_advantages"></a>Vantagens de usar os novos recursos de Gateway RAS  
A seguir estão algumas das vantagens de usar esses novos recursos de Gateway RAS ao projetar sua implantação RAS Gateway.  
  
**Dimensionamento do Gateway RAS**  
  
Como você pode adicionar quantas VMs do Gateway RAS quantos forem necessários para pools de Gateway RAS, você pode facilmente dimensionar a implantação de Gateway RAS para otimizar o desempenho e capacidade. Quando você adiciona VMs a um pool, você pode configurar esses Gateways RAS com conexões de VPN-to-site de qualquer natureza (IKEv2, L3, GRE), eliminando afunilamentos de capacidade com sem tempo de inatividade.  
  
**Gerenciamento de Gateway de Site corporativo simplificado**  
  
Quando seu locatário tem vários sites corporativos, o locatário pode configurar todos os sites com-to-site VPN endereço IP remoto e um endereço IP único vizinho remoto - seu datacenter CSP RAS Gateway BGP rota Reflector VIP para esse locatário. Isso simplifica o gerenciamento de gateway de seu locatários.  
  
**Correção rápida de falha de Gateway**  
  
Para garantir uma resposta de failover rápido, você pode configurar o tempo de parâmetro de Keep-Alive BGP entre rotas de borda e o roteador de controle para um intervalo de tempo curto, como menor ou igual a dez segundos. Com esse intervalo de curta keep alive, se um roteador de borda RAS Gateway BGP falhar, a falha é detectada rapidamente e controlador de rede segue as etapas fornecidas nas seções anteriores. Essa vantagem pode reduzir a necessidade de um protocolo de detecção de falha separado, como o protocolo de detecção de encaminhamento de bidirecional (BFD).  
  
 
  


