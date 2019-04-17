---
title: Gateway RAS alta disponibilidade
description: Você pode usar este tópico para saber mais sobre configurações de alta disponibilidade do gateway de Multitenant RAS para Software definido de rede (SDN) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 34d826c9-65bc-401f-889d-cf84e12f0144
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 48794ff8312ca00eda25f6d8bdca9929fc47084f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-high-availability"></a>Gateway RAS alta disponibilidade

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre configurações de alta disponibilidade do gateway de Multitenant RAS para Software definido de rede (SDN).  
  
Este tópico contém as seções a seguir.  
  
-   [Visão geral de Gateway RAS](#bkmk_overview)  
  
-   [Visão geral de Pools de gateway](#bkmk_pools)  
  
-   [Visão geral da implantação RAS Gateway](#bkmk_deployment)  
  
-   [Integração de Gateway RAS com o controlador de rede](#bkmk_integration)  
  
## <a name="bkmk_overview"></a>Visão geral de Gateway RAS  
Se sua organização for um provedor de serviços de nuvem (CSP) ou uma empresa com vários locatários, você pode implantar RAS Gateway no modo de vários locatários para fornecer o roteamento de tráfego de rede para e de redes físicos e virtuais, incluindo a Internet.  
  
Você pode implantar RAS Gateway no modo de vários locatários como um gateway edge rotear tráfego de rede de cliente do locatário do locatário recursos e redes virtuais.  
  
Quando você implanta várias instâncias do RAS Gateway VMs que fornecem failover e alta disponibilidade, você está implantando um pool de gateway. No Windows Server 2012 R2, o gateway todas as VMs formaram um único pool, qual fez uma separação lógica da implantação gateway um pouco difícil.  Windows Server 2012 R2 gateway oferecida uma implantação de 1:1 redundância do gateway VMs, resultando em mudanças em utilização da capacidade disponível para to-site (S2S) conexões VPN.  
  
Esse problema é resolvido no Windows Server 2016, que fornece vários grupos de Gateway - que podem ser de tipos diferentes de separação lógica. O novo modo de redundância M + N permite uma configuração de failover mais eficiente.  
  
Para obter mais informações gerais sobre o Gateway RAS, consulte [RAS Gateway](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="bkmk_pools"></a>Visão geral de Pools de gateway  
No Windows Server 2016, você pode implantar gateways em pools de um ou mais.  
  
A ilustração a seguir mostra diferentes tipos de pools de gateway que oferecem tráfego roteamento entre redes virtuais.  
  
![Pools de Gateway RAS](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Cada pool tem as seguintes propriedades.  
  
-   Cada pool é M + N redundante. Isso significa que um estou ' diversas ativa gateway VMs são copiadas por um número de'n' de gateway espera VMs. O valor de N (modo de espera gateways) é sempre menor ou igual a M (active gateways).  
  
-   Um pool pode realizar qualquer uma das funções individuais gateway - Internet Key Exchange versão 2 (IKEv2) S2S, Layer 3 (L3) e roteamento encapsulamento genérico (GRE) - ou o pool pode executar todas essas funções.  
  
-   Você pode atribuir um endereço IP público único para todos os pools de ou para um subconjunto de pools. Fazendo significativamente reduz o número de endereços IP públicos que você deve usar, porque é possível ter todos os locatários se conectar com a nuvem em um único endereço IP. A seção a seguir em alta disponibilidade e balanceamento de carga descreve como isso funciona.  
  
-   Você pode dimensionar facilmente um pool de gateway para cima ou para baixo, adicionando ou removendo gateway VMs no pool. Remoção ou a adição de gateways não interromper os serviços que são fornecidos por um pool. Você também pode adicionar e remover todos pools de gateways.  
  
-   Conexões de um único locatário podem encerrar em vários pools e vários gateways em um pool. No entanto, se um locatário tiver conexões de encerramento em um **todos os** digite pool de gateway, não podem se inscrever para outro **todos os** tipo ou pools de gateway de tipo individuais.  
  
Pools de gateway também oferecem a flexibilidade para habilitar cenários adicionais:  
  
-   Pools de locatário única - você pode criar um pool para uso por um locatário.  
  
-   Se você estiver vendendo serviços de nuvem por meio de canais de parceiro (revendedor), você pode criar conjuntos de pools separados para cada revendedor.  
  
-   Vários pools podem fornecer a mesma função de gateway mas diferentes capacidades. Por exemplo, você pode criar um pool de gateway que dá suporte a alta taxa de transferência e conexões de baixa taxa de transferência S2S IKEv2.  
  
## <a name="bkmk_deployment"></a>Visão geral da implantação RAS Gateway  
A ilustração a seguir demonstra uma típica implantação de provedor de serviços de nuvem (CSP) do RAS Gateway.  
  
![Visão geral da implantação RAS Gateway](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Com esse tipo de implantação, os pools de gateway são implantados atrás de um Software Load balanceador (SLB), que permite que o CSP atribuir um endereço IP público único para a implantação inteira. Várias conexões de gateway de um locatário podem encerrar em vários pools de gateway - e também em vários gateways dentro de um pool. Isso é mostrado por meio de conexões IKEv2 S2S no diagrama acima, mas o mesmo é aplicável a outras funções de gateway também, como gateways L3 e GRE.  
  
Na ilustração, o dispositivo MT BGP é um Gateway de Multitenant RAS com BGP. Vários locatários BGP é usado para roteamento dinâmico. O roteamento de um locatário está centralizado - um único ponto, chamado o refletor de rota (RR), lida com a correspondência de BGP para todos os sites de locatário. O registro de recurso em si é distribuído em todos os gateways em um pool. Isso resulta em uma configuração onde encerrar conexões de um locatário (caminho de dados) em vários gateways, mas o registro de recurso do locatário (ponto aos BGP - caminho do controle) é em apenas um dos gateways.  
  
O roteador BGP é separado no diagrama para descrever esse conceito de roteamento centralizado. A implementação de BGP gateway também fornece roteamento de trânsito, o que permite a nuvem atuar como um ponto de trânsito para roteamento entre sites de locatário dois. Essas funcionalidades BGP são aplicáveis a todas as funções de gateway.  
  
## <a name="bkmk_integration"></a>Integração de Gateway RAS com o controlador de rede  
Gateway RAS é totalmente integrado com o controlador de rede no Windows Server 2016. Quando Gateway RAS e controlador de rede são implantados, o controlador de rede executa as seguintes funções.  
  
-   Implantação dos pools de gateway  
  
-   Configuração de conexões de locatário em cada gateway  
  
-   Alternar o tráfego de rede flui para um modo de espera gateway no caso de uma falha de gateway  
  
As seções a seguir fornecem informações detalhadas sobre o Gateway RAS e controlador de rede.  
  
-   [Provisionamento e balanceamento de carga de conexões de Gateway (IKEv2, L3 e GRE)](#bkmk_provisioning)  
  
-   [Alta disponibilidade para IKEv2 S2S](#bkmk_ike)  
  
-   [Alta disponibilidade para GRE](#bkmk_gre)  
  
-   [Alta disponibilidade para L3 encaminhando Gateways](#bkmk_l3)  
  
### <a name="bkmk_provisioning"></a>Provisionamento e balanceamento de carga de conexões de Gateway (IKEv2, L3 e GRE)  
Quando um locatário solicita uma conexão de gateway, a solicitação é enviada para o controlador de rede. Controlador de rede está configurado com informações sobre todos os pools de gateway, incluindo a capacidade de cada pool e cada gateway em cada pool. Controlador de rede seleciona o pool correto e o gateway para a conexão. Essa seleção é baseada no requisito de largura de banda para a conexão. Controlador de rede usa um algoritmo de "melhor se ajuste" para selecionar conexões de forma eficiente em um pool. O ponto de correspondência BGP para a conexão também é designado neste momento, se esta for a primeira conexão do locatário.  
  
Depois que o controlador de rede escolhe um Gateway RAS para a conexão, o controlador de rede provisiona a configuração necessária para a conexão no gateway. Se a conexão for uma conexão S2S IKEv2, o controlador de rede também provisiona uma regra de conversão de endereços de rede (NAT) no pool de SLB; Essa regra NAT no pool de SLB direciona solicitações de conexão de locatário para o gateway designado. Locatários são diferenciados pelo IP de origem, que deve ser exclusivo.  
  
> [!NOTE]  
> Conexões L3 e GRE ignoram o SLB e conecte-se diretamente com o Gateway RAS designado.  Essas conexões exigem que o roteador de ponto de extremidade remoto (ou outro dispositivo de terceiros) deve ser configurado corretamente para conectar-se com o Gateway RAS.  
  
Se BGP roteamento é ativado para a conexão, em seguida, BGP estranhas for iniciado pelo Gateway RAS - e rotas são trocadas entre no local e na nuvem gateways. As rotas que são obtidas por BGP (ou que são configuradas estaticamente rotas se BGP não for usado) são enviadas para o controlador de rede. Controlador de rede, em seguida, plumbs as rotas até os hosts Hyper-V na qual o locatário VMs estão instalados. Neste ponto, o tráfego de locatário pode ser roteado para o site no local correto. Controlador de rede também cria políticas de virtualização do Hyper-V rede associadas que especificam locais de gateway e plumbs-las até os hosts Hyper-V.  
  
### <a name="bkmk_ike"></a>Alta disponibilidade para IKEv2 S2S  
Um Gateway RAS em um pool consiste em conexões e BGP estranhas de locatários diferentes. Cada grupo tem estou ' gateways ativos e gateways espera "n".  
  
Controlador de rede manipula a falha de gateways da seguinte maneira.  
  
-   Controlador de rede constantemente ping gateways em todos os pools e pode detecta um gateway falha ou com falha. Controlador de rede pode detectar os seguintes tipos de falhas de Gateway RAS.  
  
    -   Falha de RAS Gateway VM  
  
    -   Falha do host do Hyper-V no qual o Gateway RAS é executado  
  
    -   Falha de serviço de Gateway RAS  
  
    Controlador de rede armazena a configuração de todos os gateways active implantados. Configuração consiste em configurações de conexão e configurações de roteamento.  
  
-   Quando um gateway falha, ele afeta conexões de locatário no gateway, bem como conexões de locatário que estão localizados em outros gateways, mas cujo RR reside no gateway com falha. O tempo de inatividade das conexões de último é menor que o primeiro. Quando o controlador de rede detecta um gateway com falha, ele executa as seguintes tarefas.  
  
    -   Remove as rotas das conexões de afetados os hosts de computação.  
  
    -   Remove as políticas de virtualização do Hyper-V rede nesses hosts.  
  
    -   Seleciona um gateway no modo de espera, converte em um gateway ativo e configura o gateway.  
  
    -   Altera os mapeamentos NAT no pool de SLB para apontar conexões com o novo gateway.  
  
-   Simultaneamente, como a configuração é exibida no novo gateway ativado, as conexões de IKEv2 S2S e BGP estranhas são restabelecidas. As conexões e BGP estranhas podem ser iniciadas pelo gateway de nuvem ou o gateway de local. Os gateways atualize suas rotas e enviá-los para o controlador de rede. Depois que o controlador de rede aprende sobre as novas rotas descobertas pelos gateways, controlador de rede envia as rotas e as políticas de virtualização do Hyper-V rede associadas aos hosts Hyper-V onde residem as VMs dos locatários afetados falha. Essa atividade de rede controlador é semelhante à circunstância de uma nova configuração de conexão, ele ocorre somente em uma escala maior.  
  
### <a name="bkmk_gre"></a>Alta disponibilidade para GRE  
O processo de resposta de failover RAS Gateway pelo controlador de rede - incluindo detecção da falha, copiando conexão e roteamento configuração para o modo de espera gateway, failover de roteamento BGP/estático do afetados conexões (inclusive a retirada e novamente a estrutura de rotas em computação hosts e BGP novamente estranhas) e reconfiguração das políticas de virtualização de rede do Hyper-V nos hosts de computação - é o mesmo para conexões e gateways GRE. O estabelecimento de conexões GRE re acontece de forma diferente, no entanto, e a solução de alta disponibilidade para GRE tem alguns requisitos adicionais.  
  
![Alta disponibilidade para GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
No momento da implantação de gateway, todas as VMs do Gateway RAS é atribuída um endereço IP dinâmico (DIP). Além disso, cada gateway VM também é atribuído um endereço IP virtual (VIP) para alta disponibilidade GRE. VIPs são atribuídas apenas para gateways em pools que possam aceitar conexões de GRE e não para pools sem GRE. São anunciados VIPs atribuídos à parte superior de switches de rack (TOR) usando BGP, que ainda mais anuncia VIPs à rede física na nuvem. Isso torna os gateways acessível da roteadores remotos ou dispositivos de terceiros em que reside a outra extremidade da conexão a GRE. Este BGP estranhas é diferente de correspondências para a troca de rotas de locatário de BGP o nível de locatário.  
  
No momento do provisionamento de conexão GRE, controlador de rede seleciona um gateway, configura um ponto de extremidade GRE no gateway selecionado e retorna o endereço VIP do gateway atribuído. Este VIP, em seguida, é configurado como o endereço de túnel GRE no roteador remoto de destino.  
  
Quando um gateway falha, o controlador de rede copiará o endereço VIP do gateway com falha e outros dados de configuração para o gateway de modo de espera. Quando o modo de espera gateway estiver ativo, ele anuncia o VIP para seu comutador TOR e mais detalhes sobre a rede física. Continuam roteadores remotos conectem túneis mesmo VIP e a infraestrutura de roteamento garante que os pacotes são roteadas para o novo gateway ativo.  
  
### <a name="bkmk_l3"></a>Alta disponibilidade para L3 encaminhando Gateways  
Um gateway de encaminhamento L3 de virtualização de rede do Hyper-V é uma ponte entre a infraestrutura física no data center e a infraestrutura virtualizada na nuvem virtualização de rede do Hyper-V. Em um gateway L3 encaminhamento vários locatários, cada locatário usa sua própria rede de lógica VLAN marcado para conectividade com a rede física do locatário.  
  
Quando um novo locatário cria um novo gateway L3, Gerenciador de serviço de Gateway de controlador de rede seleciona um gateway disponível VM e configura uma nova interface de locatário com um endereço IP de espaço endereço do cliente (CA) altamente disponível (a partir VLAN marcado rede lógica do Locatário). O endereço IP é usado como o endereço IP de par no gateway remoto (rede física) e é o próximo-salto para alcançar virtualização de rede do Hyper-V rede do locatário.  
  
Diferentemente de conexões de rede IPsec ou GRE, a opção TOR não aprenderá rede marcado de VLAN do locatário dinamicamente. O roteamento de rede marcado de VLAN do locatário precisa ser configurado no switch TOR e todos os comutadores intermediários e roteadores entre infraestrutura física e o gateway para garantir a conectividade de ponta a ponta.  A seguir está um exemplo de configuração de rede Virtual CSP conforme mostrado na ilustração abaixo.  
  
|Rede|Sub-rede|ID DE VLAN|Gateway padrão|  
|-----------|----------|-----------|-------------------|  
|Rede lógica Contoso L3|10.127.134.0/24|1001|10.127.134.1|  
|Rede lógica Woodgrove L3|10.127.134.0/24|1002|10.127.134.1|  
  
A seguir é exemplos de configurações de gateway de locatário, conforme mostrado na ilustração abaixo.  
  
|Nome de locatário|Gateway L3 endereço IP|ID DE VLAN|Endereço IP Peer|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Woodgrove|10.127.134.60|1002|10.127.134.65|  
  
A seguir está a ilustração dessas configurações em um datacenter do CSP.  
  
![Alta disponibilidade para L3 encaminhando Gateways](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
As falhas de gateway, a detecção da falha e o processo de failover gateway no contexto de um L3 gateway de encaminhamento é semelhante aos processos de IKEv2 e GRE RAS Gateways. As diferenças são a maneira como os endereços IP externos são manipulados.  
  
Quando o estado VM gateway se torna não íntegro, o controlador de rede seleciona um dos gateways no modo de espera do pool e provisiona novamente as conexões de rede e roteamento no modo de espera gateway. Enquanto movendo as conexões, o gateway L3 encaminhando altamente disponível endereço IP de espaço de CA também é movido para o novo gateway VM juntamente com o espaço de CA endereço BGP IP do locatário.  
  
Como o endereço IP de estranhas L3 é movido para o novo gateway VM durante o failover, a infraestrutura de física remota novamente é capaz de se conectar a esse endereço IP e, subsequentemente, alcançar a carga de trabalho de virtualização de rede do Hyper-V. Para BGP roteamento dinâmico, como o espaço de CA endereço BGP IP é movido para o novo gateway VM, o roteador BGP remoto pode restabelecer estranhas e saiba todas as rotas de virtualização do Hyper-V rede novamente.  
  
> [!NOTE]  
> Separadamente configure as opções de TOR e todos os roteadores intermediários para usar a rede de lógica VLAN marcada para comunicação de locatário. Além disso, o failover L3 é restrito a somente os racks que são configurados dessa maneira. Por isso, o pool de gateway L3 deve ser configurado com cuidado e configuração manual deverão ser concluída separadamente.  
  


