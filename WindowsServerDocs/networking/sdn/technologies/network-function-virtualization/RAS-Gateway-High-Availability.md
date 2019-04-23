---
title: Alta disponibilidade do Gateway de RAS
description: Você pode usar este tópico para saber mais sobre configurações de alta disponibilidade para o Gateway de multilocatário de RAS para Software Defined Networking (SDN) no Windows Server 2016.
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
ms.openlocfilehash: 8ce515ceeb7ab6989ef18055f312983a6518a1a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847757"
---
# <a name="ras-gateway-high-availability"></a>Alta disponibilidade do Gateway de RAS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre configurações de alta disponibilidade para o Gateway de multilocatário de RAS para Software Defined Networking (SDN).  
  
Este tópico contém as seguintes seções.  
  
-   [Visão geral do Gateway RAS](#bkmk_overview)  
  
-   [Visão geral de Pools de gateway](#bkmk_pools)  
  
-   [Visão geral da implantação de Gateway RAS](#bkmk_deployment)  
  
-   [Integração com o controlador de rede do Gateway RAS](#bkmk_integration)  
  
## <a name="bkmk_overview"></a>Visão geral do Gateway RAS  
Se sua organização for um provedor de serviços de nuvem (CSP) ou uma empresa com vários locatários, você pode implantar o Gateway de RAS no modo multilocatário para fornecer roteamento de tráfego de rede para e de redes virtuais e físicas, incluindo a Internet.  
  
Você pode implantar o Gateway de RAS no modo multilocatário como um gateway edge para rotear o tráfego de rede de cliente do locatário para redes virtuais e recursos de locatário.  
  
Quando você implantar várias instâncias de VMs de Gateway de RAS que fornecem failover e alta disponibilidade, você está implantando um pool de gateway. No Windows Server 2012 R2, o gateway todas as VMs formaram um único pool, o que dificultava um pouco uma separação lógica da implantação do gateway.  Gateway do Windows Server 2012 R2 oferecida uma implantação de redundância de 1:1 para VMs, o que resultou na subutilização da capacidade disponível para o site a site (S2S) conexões VPN de gateway.  
  
Esse problema é resolvido no Windows Server 2016, que fornece vários Pools de Gateway - que pode ser de tipos diferentes de separação lógica. O novo modo de redundância M + N permite uma configuração de failover mais eficiente.  
  
Para obter mais informações gerais sobre o Gateway de RAS, consulte [Gateway de RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="bkmk_pools"></a>Visão geral de Pools de gateway  
No Windows Server 2016, você pode implantar gateways em um ou mais pools.  
  
A ilustração a seguir mostra os diferentes tipos de pools de gateway que fornecem o roteamento de tráfego entre redes virtuais.  
  
![Pools de Gateway de RAS](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Cada pool tem as seguintes propriedades.  
  
-   Cada pool é com redundância M + N. Isso significa que um estou ' número de VMs de gateway Active Directory seja realizado por um número de ' n' de VMs de gateway em espera. O valor de N (gateways em espera) é sempre menor ou igual a M (gateways Active Directory).  
  
-   Um pool pode executar qualquer uma das funções individuais de gateway: protocolo IKE versão 2 S2S de (IKEv2), camada 3 (L3) e Generic Routing Encapsulation (GRE) - ou o pool pode executar todas essas funções.  
  
-   Você pode atribuir um único endereço IP público para todos os pools ou para um subconjunto dos pools. Isso diminuirá bastante o número de endereços IP públicos que você deve usar, porque é possível ter todos os locatários a conectar-se para a nuvem em um único endereço IP. A seção abaixo sobre alta disponibilidade e balanceamento de carga descreve como isso funciona.  
  
-   Você pode facilmente dimensionar um pool de gateway para cima ou para baixo, adicionando ou removendo as VMs de gateway no pool. Remoção ou adição de gateways não interrompa os serviços que são fornecidos por um pool. Você também pode adicionar e remover pools inteiros de gateways.  
  
-   Conexões de um único locatário podem terminar em vários pools e vários gateways em um pool. No entanto, se um locatário tiver conexões terminando em um **todos os** tipo de pool de gateway, ele não é possível assinar outros **todos os** pools de gateway do tipo individual ou tipo.  
  
Pools de gateway também fornecem a flexibilidade para habilitar cenários adicionais:  
  
-   Pools de locatário único – você pode criar um pool para uso por um locatário.  
  
-   Se você estiver vendendo serviços de nuvem por meio de canais de parceiros (revendedores), você pode criar conjuntos separados de pools para cada revendedor.  
  
-   Vários pools podem fornecer a mesma função de gateway, mas as capacidades diferentes. Por exemplo, você pode criar um pool de gateway que dá suporte a conexões de baixa taxa de transferência IKEv2 S2S e de alta taxa de transferência.  
  
## <a name="bkmk_deployment"></a>Visão geral da implantação de Gateway RAS  
A ilustração a seguir demonstra uma implantação típica do provedor de serviços de nuvem (CSP) do Gateway de RAS.  
  
![Visão geral da implantação de Gateway RAS](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Com esse tipo de implantação, os pools de gateway são implantados por trás de um Software SLB balanceador de carga (), que permite que o CSP atribuir um único endereço IP público para toda a implantação. Várias conexões de gateway de um locatário podem terminar em vários pools de gateway – e também em vários gateways dentro de um pool. Isso é ilustrado por meio de conexões S2S de IKEv2 no diagrama acima, mas o mesmo é aplicável a outras funções de gateway também, como gateways L3 e GRE.  
  
Na ilustração, o dispositivo de MT BGP é um Gateway de multilocatário de RAS com BGP. BGP multilocatário é usado para roteamento dinâmico. O roteamento para um locatário é centralizado - um único ponto, chamado de refletor de rota (RR), lida com o emparelhamento via protocolo BGP para todos os sites de locatário. O registro de recurso em si é distribuído entre todos os gateways em um pool. Isso resulta em uma configuração em que as conexões de um locatário (caminho de dados) terminam em vários gateways, mas o registro de recurso para o locatário (ponto de emparelhamento de BGP - caminho de controle) está em apenas um dos gateways.  
  
O roteador BGP é separado do diagrama para descrever esse conceito de roteamento centralizado. A implementação de protocolo BGP de gateway também oferece roteamento de tráfego, o que permite que a nuvem atuar como um ponto de trânsito para o roteamento entre os sites de locatário de dois. Esses recursos BGP são aplicáveis a todas as funções de gateway.  
  
## <a name="bkmk_integration"></a>Integração com o controlador de rede do Gateway RAS  
Gateway de RAS é totalmente integrado com o controlador de rede no Windows Server 2016. Quando o Gateway de RAS e controlador de rede são implantados, o controlador de rede realiza as seguintes funções.  
  
-   Implantação de pools de gateway  
  
-   Configuração de conexões de locatário em cada gateway  
  
-   Alternar o tráfego de rede fluem para um gateway em espera em caso de falha do gateway  
  
As seções a seguir fornecem informações detalhadas sobre o Gateway de RAS e controlador de rede.  
  
-   [Provisionamento e balanceamento de carga das conexões de Gateway (IKEv2, L3 e GRE)](#bkmk_provisioning)  
  
-   [Alta disponibilidade para IKEv2 S2S](#bkmk_ike)  
  
-   [Alta disponibilidade do GRE](#bkmk_gre)  
  
-   [Alta disponibilidade para L3 Gateways de encaminhamento](#bkmk_l3)  
  
### <a name="bkmk_provisioning"></a>Provisionamento e balanceamento de carga das conexões de Gateway (IKEv2, L3 e GRE)  
Quando um locatário solicita uma conexão de gateway, a solicitação é enviada para o controlador de rede. Controlador de rede é configurado com as informações sobre todos os pools de gateway, incluindo a capacidade de cada pool e cada gateway em cada pool. Controlador de rede seleciona o pool corretos e o gateway para a conexão. Esta seleção se baseia o requisito de largura de banda para a conexão. Controlador de rede usa um algoritmo de "melhor ajuste" para escolher as conexões com eficiência em um pool. O ponto de emparelhamento via protocolo BGP para a conexão também é designado no momento, se esta for a primeira conexão do locatário.  
  
Depois que o controlador de rede seleciona um Gateway de RAS para a conexão, o controlador de rede provisiona a configuração necessária para a conexão no gateway. Se a conexão for uma conexão S2S IKEv2, o controlador de rede também provisiona uma regra de conversão de endereço de rede (NAT) no pool de SLB; Essa regra NAT no pool de SLB direciona solicitações de conexão do locatário para o gateway designado. Locatários são diferenciados pelo IP de origem, que deve ser exclusivo.  
  
> [!NOTE]  
> Conexões L3 e GRE ignoram o SLB e conectar-se diretamente com o Gateway de RAS designado.  Essas conexões exigem que o roteador de ponto de extremidade remoto (ou outro dispositivo de terceiros) deve ser configurado corretamente para se conectar com o Gateway de RAS.  
  
Se o roteamento de BGP está habilitado para a conexão, em seguida, emparelhamento via protocolo BGP é iniciado pelo Gateway de RAS – e as rotas são trocadas entre os locais e na nuvem gateways. As rotas que são aprendidos pelo protocolo BGP (ou que são configuradas estaticamente rotas se BGP não for usado) são enviadas para o controlador de rede. Controlador de rede, em seguida, conecta as rotas para baixo até os hosts do Hyper-V no qual as VMs do locatário estão instalados. Neste ponto, o tráfego de locatário pode ser roteado para o site no local correto. Controlador de rede também cria políticas de virtualização de rede do Hyper-V associadas que especificam locais de gateway e conecta-os para baixo até os hosts Hyper-V.  
  
### <a name="bkmk_ike"></a>Alta disponibilidade para IKEv2 S2S  
Um Gateway de RAS em um pool consiste em conexões e o emparelhamento via protocolo BGP de locatários diferentes. Cada pool tem estou ' gateways ativos e gateways em espera de ' n'.  
  
Controlador de rede processa a falha dos gateways da seguinte maneira.  
  
-   Controlador de rede constantemente pings de gateways em todos os pools e pode detectam um gateway que falhou ou com falha. Controlador de rede pode detectar os seguintes tipos de falhas de Gateway de RAS.  
  
    -   Falha da VM de Gateway de RAS  
  
    -   Falha do host do Hyper-V no qual o Gateway de RAS está em execução.  
  
    -   Falha do serviço de Gateway de RAS  
  
    Controlador de rede armazena a configuração de todos os gateways ativos implantados. Configuração consiste em configurações de conexão e roteamento.  
  
-   Quando um gateway falha, ele afeta as conexões de locatário no gateway, bem como conexões de locatário que estão localizados em outros gateways, mas cujo RR reside no gateway com falha. O tempo de inatividade das conexões de último é menor que o primeiro. Quando o controlador de rede detecta um gateway com falha, ele executa as seguintes tarefas.  
  
    -   Remove as rotas de conexões afetadas os hosts de computação.  
  
    -   Remove as políticas de virtualização de rede do Hyper-V nesses hosts.  
  
    -   Seleciona um gateway em espera, converte-o em um gateway de Active Directory e configura o gateway.  
  
    -   Altera os mapeamentos NAT no pool de SLB ao ponto de conexões para o novo gateway.  
  
-   Ao mesmo tempo, como a configuração é exibido no novo gateway Active Directory, as conexões S2S IKEv2 e o emparelhamento via protocolo BGP são restabelecidas. As conexões e o emparelhamento via protocolo BGP podem ser iniciadas pelo gateway de nuvem ou o gateway local. Os gateways de atualizar suas rotas e enviá-los para o controlador de rede. Depois que o controlador de rede aprende as rotas de novo descobertas pelos gateways, controlador de rede envia as rotas e as políticas de virtualização de rede do Hyper-V associadas aos hosts do Hyper-V onde residem as VMs afetadas por falha de locatários. Esta atividade de controlador de rede é semelhante ao caso de uma nova configuração de conexão, ele ocorre apenas em uma escala maior.  
  
### <a name="bkmk_gre"></a>Alta disponibilidade do GRE  
O processo de resposta de failover do Gateway de RAS por controlador de rede – incluindo a detecção de falha, copiando a conexão e roteamento de configuração para o gateway em espera, o failover de roteamento de BGP/estática das conexões afetadas (incluindo a retirada e detalhes técnicos novamente de rotas na computação novamente emparelhamento via protocolo BGP e hosts), e reconfiguração de políticas de virtualização de rede do Hyper-V em hosts de computação - é o mesmo para conexões e gateways GRE. O restabelecimento de conexões de GRE acontece de forma diferente, no entanto, e a solução de alta disponibilidade para GRE tem alguns requisitos adicionais.  
  
![Alta disponibilidade do GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
No momento da implantação do gateway, todas as VMs de Gateway de RAS é atribuída um endereço IP dinâmico (DIP). Além disso, cada VM de gateway também é atribuído um endereço IP virtual (VIP) para alta disponibilidade GRE. VIPs são atribuídos somente para gateways nos pools que podem aceitar conexões de GRE e não para os pools não GRE. Os VIP atribuídos são anunciados na parte superior do switches de rack (TOR) usando o BGP, que anuncia ainda mais os VIPs para a rede física de nuvem. Isso torna os gateways pode ser acessado do remotos roteadores ou dispositivos de terceiros em que reside a outra extremidade da conexão de GRE. Esse emparelhamento via protocolo BGP é diferente do nível de locatário emparelhamento BGP para a troca de rotas de locatário.  
  
No momento do provisionamento da conexão de GRE, controlador de rede seleciona um gateway, configura um ponto de extremidade GRE no gateway selecionado e retorna o endereço VIP do gateway atribuído. Este VIP, em seguida, é configurado como o endereço de túnel GRE no roteador remoto de destino.  
  
Quando um gateway falha, o controlador de rede copia o endereço VIP do gateway com falha e outros dados de configuração para o gateway em espera. Quando o gateway em espera se torna ativo, ele anuncia o VIP para seu comutador TOR e mais detalhadamente a rede física. Roteadores remotos continuam a ser conectadas a túneis GRE para o mesmo VIP e a infraestrutura de roteamento garante que os pacotes são roteados para o novo gateway ativo.  
  
### <a name="bkmk_l3"></a>Alta disponibilidade para L3 Gateways de encaminhamento  
Um gateway de encaminhamento L3 de virtualização de rede do Hyper-V é uma ponte entre a infraestrutura física no datacenter e a infraestrutura virtualizada na nuvem virtualização de rede do Hyper-V. Em um gateway de encaminhamento L3 multilocatário, cada locatário usa sua própria rede lógica com marcas de formatação de VLAN para conectividade de rede de física do locatário.  
  
Quando um novo locatário cria um novo gateway L3, Gerenciador de Gateway do serviço do controlador de rede seleciona um VM de gateway disponível e configura uma nova interface de locatário com um endereço IP de espaço em endereço de cliente (CA) altamente disponível (da rede lógica com marcas de formatação de VLAN do locatário ). O endereço IP é usado como o endereço IP do par no gateway remoto (rede física) e é o próximo salto para alcançar a rede de virtualização de rede do Hyper-V do locatário.  
  
Ao contrário do IPsec ou GRE conexões de rede, o comutador TOR não aprenderá rede marcada de VLAN do locatário dinamicamente. O roteamento de rede com marcas de formatação de VLAN do locatário precisa ser configurado no comutador TOR e todos os intermediários comutadores e roteadores entre a infraestrutura física e o gateway para garantir a conectividade de ponta a ponta.  A seguir está um exemplo de configuração de rede Virtual do CSP, conforme mostrado na ilustração abaixo.  
  
|Rede|Sub-rede|ID DA VLAN|Gateway Padrão|  
|-----------|----------|-----------|-------------------|  
|Rede lógica L3 da Contoso|10.127.134.0/24|1001|10.127.134.1|  
|Rede lógica Woodgrove L3|10.127.134.0/24|1002|10.127.134.1|  
  
A seguir é exemplos de configurações de gateway de locatário conforme mostrado na ilustração abaixo.  
  
|Nome do locatário|Endereço IP do gateway L3|ID DA VLAN|Endereço IP do par|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Woodgrove|10.127.134.60|1002|10.127.134.65|  
  
A seguir está a ilustração dessas configurações em um datacenter do CSP.  
  
![Alta disponibilidade para L3 Gateways de encaminhamento](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
As falhas de gateway, detecção de falha e o processo de failover do gateway no contexto de um L3 no gateway de encaminhamento é semelhante aos processos para IKEv2 e Gateways de RAS do GRE. As diferenças são a maneira como os endereços IP externos são manipulados.  
  
Quando o estado da VM de gateway se torna não íntegro, o controlador de rede seleciona um dos gateways em espera do pool e provisiona novamente as conexões de rede e roteamento no gateway em espera. Enquanto as conexões, o gateway de encaminhamento L3 altamente disponível endereço IP de espaço de CA também é movido para o novo gateway VM junto com o espaço do CA do endereço IP do BGP do locatário.  
  
Como o endereço IP de emparelhamento de L3 é movido para o novo gateway VM durante o failover, a infraestrutura física remota novamente é capaz de se conectar a esse endereço IP e, posteriormente, acessar a carga de trabalho de virtualização de rede do Hyper-V. Para BGP roteamento dinâmico, como o espaço do CA do endereço IP do BGP é movido para o novo gateway de VM, o roteador BGP remoto pode restabelecer o emparelhamento e saber todas as rotas de virtualização de rede do Hyper-V novamente.  
  
> [!NOTE]  
> Você deve configurar separadamente os comutadores TOR e todos os roteadores intermediários para usar a rede lógica com marcas de formatação de VLAN para a comunicação de locatário. Além disso, o failover de L3 é restrito aos racks configurados dessa forma. Por isso, o pool de gateway L3 deve ser configurado com cuidado e a configuração manual deve ser concluída separadamente.  
  


