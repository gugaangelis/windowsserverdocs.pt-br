---
title: Alta disponibilidade do Gateway de RAS
description: Você pode usar este tópico para saber mais sobre as configurações de alta disponibilidade para o gateway de multilocatário do RAS para SDN (rede definida pelo software) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 34d826c9-65bc-401f-889d-cf84e12f0144
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5fca4fc6a636bcde155e60b6da3c827bc9313606
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313041"
---
# <a name="ras-gateway-high-availability"></a>Alta disponibilidade do Gateway de RAS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre as configurações de alta disponibilidade para o gateway de multilocatário do RAS para SDN (rede definida pelo software).  
  
Este tópico contém as seguintes seções.  
  
-   [Visão geral do gateway de RAS](#bkmk_overview)  
  
-   [Visão geral de pools de gateway](#bkmk_pools)  
  
-   [Visão geral da implantação do gateway de RAS](#bkmk_deployment)  
  
-   [Integração do gateway de RAS com o controlador de rede](#bkmk_integration)  
  
## <a name="ras-gateway-overview"></a><a name="bkmk_overview"></a>Visão geral do gateway de RAS  
Se sua organização for um CSP (provedor de serviços de nuvem) ou uma empresa com vários locatários, você poderá implantar o gateway de RAS no modo multilocatário para fornecer roteamento de tráfego de rede de e para redes físicas e virtuais, incluindo a Internet.  
  
Você pode implantar o gateway RAS no modo multilocatário como um gateway de borda para rotear o tráfego de rede do cliente de locatário para redes virtuais de locatário e recursos.  
  
Ao implantar várias instâncias de VMs de gateway de RAS que fornecem alta disponibilidade e failover, você está implantando um pool de gateway. No Windows Server 2012 R2, todas as VMs de gateway formaram um único pool, o que tornava uma separação lógica da implantação do gateway um pouco difícil.  O gateway do Windows Server 2012 R2 ofereceu uma implantação de redundância 1:1 para as VMs de gateway, o que resultou na utilização da capacidade disponível para conexões VPN site a site (S2S).  
  
Esse problema é resolvido no Windows Server 2016, que fornece vários pools de gateways, que podem ser de tipos diferentes para separação lógica. O novo modo de redundância de M + N permite uma configuração de failover mais eficiente.  
  
Para obter mais informações gerais sobre o gateway RAS, consulte [Gateway de Ras](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="gateway-pools-overview"></a><a name="bkmk_pools"></a>Visão geral de pools de gateway  
No Windows Server 2016, você pode implantar gateways em um ou mais pools.  
  
A ilustração a seguir mostra diferentes tipos de pools de gateway que fornecem roteamento de tráfego entre redes virtuais.  
  
![Pools de gateway de RAS](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Cada pool tem as seguintes propriedades.  
  
-   Cada pool é M + N redundante. Isso significa que o backup de um ' M' de VMs de gateway ativo é feito por um número ' N ' de VMs de gateway em espera. O valor de N (gateways em espera) é sempre menor ou igual a M (gateways ativos).  
  
-   Um pool pode executar qualquer uma das funções de gateway individuais – protocolo IKE versão 2 (IKEv2) S2S, camada 3 (L3) e encapsulamento de roteamento genérico (GRE) – ou o pool pode executar todas essas funções.  
  
-   Você pode atribuir um único endereço IP público a todos os pools ou a um subconjunto de pools. Isso reduz significativamente o número de endereços IP públicos que você deve usar, pois é possível que todos os locatários se conectem à nuvem em um único endereço IP. A seção abaixo sobre alta disponibilidade e balanceamento de carga descreve como isso funciona.  
  
-   Você pode facilmente dimensionar um pool de gateway para cima ou para baixo adicionando ou removendo VMs de gateway no pool. A remoção ou a adição de gateways não interrompe os serviços fornecidos por um pool. Você também pode adicionar e remover pools inteiros de gateways.  
  
-   As conexões de um único locatário podem terminar em vários pools e vários gateways em um pool. No entanto, se um locatário tiver conexões encerradas em um pool de gateway de **todos os** tipos, ele não poderá assinar outros pools de gateway de tipo **qualquer** ou tipo individual.  
  
Os pools de gateway também fornecem a flexibilidade para habilitar cenários adicionais:  
  
-   Pools de locatário único – você pode criar um pool para ser usado por um locatário.  
  
-   Se você estiver vendendo serviços de nuvem por meio de canais de parceiro (revendedor), poderá criar conjuntos separados de pools para cada revendedor.  
  
-   Vários pools podem fornecer a mesma função de gateway, mas diferentes capacidades. Por exemplo, você pode criar um pool de gateway que dá suporte a altas taxas de transferência e conexões S2S de baixa taxa de transferência.  
  
## <a name="ras-gateway-deployment-overview"></a><a name="bkmk_deployment"></a>Visão geral da implantação do gateway de RAS  
A ilustração a seguir demonstra uma implantação típica do CSP (provedor de serviços de nuvem) do gateway de RAS.  
  
![Visão geral da implantação do gateway de RAS](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Com esse tipo de implantação, os pools de gateway são implantados por trás de um Load Balancer de software (SLB), que permite ao CSP atribuir um único endereço IP público para toda a implantação. Várias conexões de gateway de um locatário podem terminar em vários pools de gateway – e também em vários gateways em um pool. Isso é ilustrado por meio de conexões S2S IKEv2 no diagrama acima, mas o mesmo também se aplica a outras funções de gateway, como os gateways L3 e GRE.  
  
Na ilustração, o dispositivo BGP do MT é um gateway de multilocatário do RAS com BGP. O BGP multilocatário é usado para roteamento dinâmico. O roteamento de um locatário é centralizado – um ponto único, chamado de RR (refletor de rota), manipula o emparelhamento via protocolo BGP para todos os sites de locatário. O próprio RR é distribuído entre todos os gateways em um pool. Isso resulta em uma configuração em que as conexões de um locatário (caminho de dados) são encerradas em vários gateways, mas o RR para o locatário (ponto de emparelhamento BGP-caminho de controle) está em apenas um dos gateways.  
  
O roteador BGP é separado no diagrama para representar esse conceito de roteamento centralizado. A implementação BGP de gateway também fornece roteamento de trânsito, que permite que a nuvem atue como um ponto de trânsito para roteamento entre dois sites de locatário. Esses recursos de BGP são aplicáveis a todas as funções de gateway.  
  
## <a name="ras-gateway-integration-with-network-controller"></a><a name="bkmk_integration"></a>Integração do gateway de RAS com o controlador de rede  
O gateway de RAS é totalmente integrado ao controlador de rede no Windows Server 2016. Quando o gateway RAS e o controlador de rede são implantados, o controlador de rede executa as seguintes funções.  
  
-   Implantação dos pools de gateway  
  
-   Configuração de conexões de locatário em cada gateway  
  
-   Alternar fluxos de tráfego de rede para um gateway em espera no caso de uma falha de gateway  
  
As seções a seguir fornecem informações detalhadas sobre o gateway RAS e o controlador de rede.  
  
-   [Provisionamento e balanceamento de carga de conexões de gateway (IKEv2, L3 e GRE)](#bkmk_provisioning)  
  
-   [Alta disponibilidade para S2S IKEv2](#bkmk_ike)  
  
-   [Alta disponibilidade para GRE](#bkmk_gre)  
  
-   [Alta disponibilidade para gateways de encaminhamento L3](#bkmk_l3)  
  
### <a name="provisioning-and-load-balancing-of-gateway-connections-ikev2-l3-and-gre"></a><a name="bkmk_provisioning"></a>Provisionamento e balanceamento de carga de conexões de gateway (IKEv2, L3 e GRE)  
Quando um locatário solicita uma conexão de gateway, a solicitação é enviada ao controlador de rede. O controlador de rede é configurado com informações sobre todos os pools de gateway, incluindo a capacidade de cada pool e todos os gateways em cada pool. Controlador de rede seleciona o pool e o gateway corretos para a conexão. Essa seleção se baseia no requisito de largura de banda para a conexão. O controlador de rede usa um algoritmo "melhor ajuste" para escolher conexões com eficiência em um pool. O ponto de emparelhamento via protocolo BGP para a conexão também é designado neste momento se esta for a primeira conexão do locatário.  
  
Depois que o controlador de rede selecionar um gateway RAS para a conexão, o controlador de rede provisionará a configuração necessária para a conexão no gateway. Se a conexão for uma conexão S2S IKEv2, o controlador de rede também provisionará uma regra NAT (conversão de endereços de rede) no pool SLB; Essa regra NAT no pool SLB direciona as solicitações de conexão do locatário para o gateway designado. Os locatários são diferenciados pelo IP de origem, que deve ser exclusivo.  
  
> [!NOTE]  
> As conexões L3 e GRE ignoram o SLB e se conectam diretamente com o gateway RAS designado.  Essas conexões exigem que o roteador de ponto de extremidade remoto (ou outro dispositivo de terceiros) esteja configurado corretamente para se conectar ao gateway RAS.  
  
Se o roteamento BGP estiver habilitado para a conexão, o emparelhamento via protocolo BGP será iniciado pelo gateway de RAS e as rotas serão trocadas entre os gateways locais e de nuvem. As rotas aprendidas pelo BGP (ou que são rotas configuradas estaticamente se o BGP não é usado) são enviadas ao controlador de rede. O controlador de rede, em seguida, direciona as rotas para os hosts do Hyper-V nos quais as VMs de locatário estão instaladas. Neste ponto, o tráfego do locatário pode ser roteado para o site local correto. O controlador de rede também cria políticas de virtualização de rede Hyper-V associadas que especificam locais de gateway e as direciona para os hosts Hyper-V.  
  
### <a name="high-availability-for-ikev2-s2s"></a><a name="bkmk_ike"></a>Alta disponibilidade para S2S IKEv2  
Um gateway de RAS em um pool consiste em conexões e no emparelhamento via protocolo BGP de locatários diferentes. Todos os pools têm ' gateways ativos e ' n' gateways em espera.  
  
O controlador de rede manipula a falha dos gateways da seguinte maneira.  
  
-   O controlador de rede executa pings em todos os gateways constantemente em todos os pools e pode detectar um gateway com falha ou falha. O controlador de rede pode detectar os seguintes tipos de falhas de gateway de RAS.  
  
    -   Falha na VM do gateway de RAS  
  
    -   Falha do host Hyper-V no qual o gateway RAS está em execução  
  
    -   Falha do serviço de gateway RAS  
  
    O controlador de rede armazena a configuração de todos os gateways ativos implantados. A configuração consiste em configurações de conexão e configurações de roteamento.  
  
-   Quando um gateway falha, ele afeta as conexões de locatário no gateway, bem como as conexões de locatário que estão localizadas em outros gateways, mas cujo RR reside no gateway com falha. O tempo de inatividade das últimas conexões é menor que o anterior. Quando o controlador de rede detecta um gateway com falha, ele executa as seguintes tarefas.  
  
    -   Remove as rotas das conexões afetadas dos hosts de computação.  
  
    -   Remove as políticas de virtualização de rede do Hyper-V nesses hosts.  
  
    -   Seleciona um gateway em espera, converte-o em um gateway ativo e configura o gateway.  
  
    -   Altera os mapeamentos NAT no pool SLB para apontar conexões para o novo gateway.  
  
-   Simultaneamente, à medida que a configuração é exibida no novo gateway ativo, as conexões S2S IKEv2 e o emparelhamento via protocolo BGP são restabelecidas. As conexões e o emparelhamento via protocolo BGP podem ser iniciados pelo gateway de nuvem ou pelo gateway local. Os gateways atualizam suas rotas e as enviam para o controlador de rede. Depois que o controlador de rede aprende as novas rotas descobertas pelos gateways, o controlador de rede envia as rotas e as políticas de virtualização de rede do Hyper-V associadas aos hosts do Hyper-V onde residem as VMs dos locatários afetados pela falha. Essa atividade do controlador de rede é semelhante à circunstância de uma nova configuração de conexão, mas só ocorre em uma escala maior.  
  
### <a name="high-availability-for-gre"></a><a name="bkmk_gre"></a>Alta disponibilidade para GRE  
O processo de resposta de failover do gateway de RAS por controlador de rede-incluindo detecção de falha, copiar a configuração de conexão e roteamento para o gateway em espera, o failover do roteamento de BGP/estático das conexões afetadas (incluindo a retirada e o redirecionamento de rotas em hosts de computação e reemparelhamento de BGP) e a reconfiguração de políticas de virtualização de rede do Hyper-V em hosts de computação-é a mesma para gateways e conexões GRE. No entanto, a redefinição de conexões GRE acontece de modo diferente, e a solução de alta disponibilidade para o GRE tem alguns requisitos adicionais.  
  
![Alta disponibilidade para GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
No momento da implantação do gateway, cada VM de gateway de RAS recebe um endereço IP dinâmico (DIP). Além disso, cada VM de gateway também recebe um endereço IP virtual (VIP) para alta disponibilidade do GRE. VIPs são atribuídos somente a gateways em pools que podem aceitar conexões GRE e não a pools não-GRE. Os VIPs atribuídos são anunciados para a parte superior dos comutadores de rack (TOR) usando o BGP, que anuncia ainda mais os VIPs para a rede física de nuvem. Isso torna os gateways acessíveis a partir de roteadores remotos ou dispositivos de terceiros em que a outra extremidade da conexão GRE reside. Esse emparelhamento via protocolo BGP é diferente do emparelhamento via protocolo BGP no nível de locatário para a troca de rotas de locatário.  
  
No momento do provisionamento de conexão GRE, o controlador de rede seleciona um gateway, configura um ponto de extremidade GRE no gateway selecionado e retorna o endereço VIP do gateway atribuído. Esse VIP é então configurado como o endereço do túnel GRE de destino no roteador remoto.  
  
Quando um gateway falha, o controlador de rede copia o endereço VIP do gateway com falha e outros dados de configuração para o gateway em espera. Quando o gateway em espera se torna ativo, ele anuncia o VIP para seu comutador TOR e ainda mais na rede física. Roteadores remotos continuam conectando túneis GRE ao mesmo VIP e a infraestrutura de roteamento garante que os pacotes sejam roteados para o novo gateway ativo.  
  
### <a name="high-availability-for-l3-forwarding-gateways"></a><a name="bkmk_l3"></a>Alta disponibilidade para gateways de encaminhamento L3  
Um gateway de encaminhamento L3 de virtualização de rede Hyper-V é uma ponte entre a infraestrutura física no datacenter e a infraestrutura virtualizada na nuvem de virtualização de rede Hyper-V. Em um gateway de encaminhamento L3 multilocatário, cada locatário usa sua própria rede lógica marcada por VLAN para conectividade com a rede física do locatário.  
  
Quando um novo locatário cria um novo gateway L3, o gateway do controlador de rede Service Manager seleciona uma VM de gateway disponível e configura uma nova interface de locatário com um endereço IP de espaço de AC (endereço de cliente) altamente disponível (da rede lógica marcada pela VLAN do locatário ). O endereço IP é usado como o endereço IP do par no gateway remoto (rede física) e é o próximo salto para alcançar a rede de virtualização de rede do Hyper-V do locatário.  
  
Ao contrário das conexões de rede IPsec ou GRE, a opção TOR não aprenderá dinamicamente a rede marcada de VLAN do locatário. O roteamento para a rede marcada por VLAN do locatário precisa ser configurado no comutador TOR e todos os comutadores intermediários e roteadores entre a infraestrutura física e o gateway para garantir a conectividade de ponta a ponta.  A seguir está um exemplo de configuração de rede virtual do CSP, conforme ilustrado na ilustração a seguir.  
  
|Rede|Sub-rede|ID DA VLAN|Gateway Padrão|  
|-----------|----------|-----------|-------------------|  
|Rede lógica contoso L3|10.127.134.0/24|1001|10.127.134.1|  
|Rede lógica do Woodgrove L3|10.127.134.0/24|1002|10.127.134.1|  
  
Veja a seguir exemplos de configurações de gateway de locatário, conforme ilustrado na ilustração a seguir.  
  
|Nome do inquilino|Endereço IP do gateway L3|ID DA VLAN|Endereço IP do par|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Woodgrove|10.127.134.60|1002|10.127.134.65|  
  
A seguir está a ilustração dessas configurações em um datacenter CSP.  
  
![Alta disponibilidade para gateways de encaminhamento L3](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
As falhas de gateway, detecção de falha e o processo de failover de gateway no contexto de um gateway de encaminhamento L3 são semelhantes aos processos para gateways de RAS IKEv2 e GRE. As diferenças estão no modo como os endereços IP externos são manipulados.  
  
Quando o estado da VM do gateway se tornar não íntegro, o controlador de rede selecionará um dos gateways em espera do pool e reprovisionará as conexões de rede e o roteamento no gateway em espera. Ao mover as conexões, o endereço IP do espaço de CA altamente disponível do gateway de encaminhamento L3 também é movido para a nova VM do gateway junto com o endereço IP do BGP do espaço da autoridade de certificação do locatário.  
  
Como o endereço IP de emparelhamento L3 é movido para a nova VM de gateway durante o failover, a infraestrutura física remota é novamente capaz de se conectar a esse endereço IP e, posteriormente, alcançar a carga de trabalho de virtualização de rede Hyper-V. Para roteamento dinâmico de BGP, como o endereço IP de BGP de espaço de CA é movido para a nova VM de gateway, o roteador BGP remoto pode restabelecer o emparelhamento e aprender todas as rotas de virtualização de rede Hyper-V novamente.  
  
> [!NOTE]  
> Você deve configurar separadamente os comutadores TOR e todos os roteadores intermediários para usar a rede lógica marcada pela VLAN para comunicação de locatário. Além disso, o failover L3 é restrito apenas aos racks configurados dessa maneira. Por isso, o pool de gateway L3 deve ser configurado cuidadosamente e a configuração manual deve ser concluída separadamente.  
  


