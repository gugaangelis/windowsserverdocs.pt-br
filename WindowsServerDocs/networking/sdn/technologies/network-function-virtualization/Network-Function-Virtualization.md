---
title: Virtualização de função de rede
description: Você pode usar este tópico para saber mais sobre a virtualização de função de rede, que permite implantar dispositivos de rede virtual como firewall de datacenter, gateway de RAS multilocatário e SLB (balanceamento de carga de software) no Windows Server 2016.
manager: grcusanz
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 33d0c9bcde79b6f1fd74de03abfce3fca26a378f
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991311"
---
# <a name="network-function-virtualization"></a>Virtualização de função de rede

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre a virtualização de função de rede, que permite implantar dispositivos de rede virtual, como o Firewall do datacenter, o gateway de RAS multilocatário e o multiplexador SLB de balanceamento de carga de software \( \) \( MUX \) .

>[!NOTE]
>Além deste tópico, a documentação de virtualização de função de rede a seguir está disponível.
> - [Visão geral do firewall do datacenter](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)
> - [Gateway de RAS para SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
> - [SLB (balanceamento de carga do software) para SDN](./software-load-balancing-for-sdn.md)

Nos data centers definidos pelo software de hoje, as funções de rede que estão sendo executadas por dispositivos de hardware (como balanceadores de carga, firewalls, roteadores, comutadores etc.) são cada vez mais virtualizadas como dispositivos virtuais. Essa "virtualização da função de rede" é uma progressão natural de virtualização do servidor e de virtualização de rede. Os dispositivos virtuais estão surgindo rapidamente e criando um mercado totalmente novo. Eles continuam gerando interesse e conquistam impulso nas plataformas de virtualização e nos serviços de nuvem.

A Microsoft incluiu um gateway autônomo como um dispositivo virtual a partir do Windows Server 2012 R2. Para obter mais informações, consulte [Gateway do Windows Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn313101(v=ws.11)). Agora, com o Windows Server 2016, a Microsoft continua a expandir e investir no mercado de virtualização de funções de rede.

## <a name="virtual-appliance-benefits"></a>Benefícios da solução de virtualização
Uma solução de virtualização é dinâmica e fácil de alterar, pois é uma máquina virtual predefinida e personalizada. Pode ser uma ou mais máquinas virtuais empacotadas, atualizadas e mantidas como uma unidade. Junto com a SDN (rede definida pelo software), você obtém a agilidade e a flexibilidade necessárias na infraestrutura atual baseada em nuvem. Por exemplo:

-   O SDN apresenta a rede como um recurso dinâmico e em pool.

-   O SDN facilita o isolamento do locatário.

-   O SDN maximiza a escala e o desempenho.

-   Os dispositivos virtuais permitem a expansão da capacidade e a mobilidade da carga de trabalho.

-   Os dispositivos virtuais minimizam a complexidade operacional.

-   Os dispositivos virtuais permitem que os clientes adquiram, implantem e gerenciem soluções previamente integradas.

    -   Os clientes podem facilmente mover o dispositivo virtual para qualquer lugar na nuvem.

    -   Os clientes podem dimensionar dispositivos virtuais vertical ou horizontalmente com base na demanda.

Para obter mais informações sobre o Microsoft SDN, consulte [rede definida pelo software](../../software-defined-networking.md).

### <a name="what-network-functions-are-being-virtualized"></a>Quais funções de rede estão sendo virtualizadas?
O Marketplace para funções de rede virtualizadas está crescendo rapidamente. As seguintes funções de rede estão sendo virtualizadas:

-   **Segurança**

    -   Firewall

    -   Antivírus

    -   DDoS (negação de serviço distribuída)

    -   IPS/IDS (sistema de prevenção de intrusão/sistemas de detecção de intrusão)

-   **Otimizadores de aplicativo/WAN**

-   **Microsoft Edge**

    -   Gateway site a site

    -   Gateways L3

    -   Roteadores

    -   Opções

    -   NAT

    -   Balanceadores de carga (não necessariamente na borda)

    -   Proxy HTTP

## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Por que a Microsoft é uma excelente plataforma para dispositivos virtuais
![Pilha de rede virtual](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)

A plataforma da Microsoft foi projetada para ser uma ótima plataforma para criar e implantar dispositivos virtuais. Eis o motivo:

-   A Microsoft fornece as principais funções de rede virtualizadas com o Windows Server 2016.

-   Você pode implantar um dispositivo virtual do fornecedor de sua escolha.

-   Você pode implantar, configurar e gerenciar seus dispositivos virtuais com o controlador de rede da Microsoft que vem com o Windows Server 2016. Para obter mais informações sobre o controlador de rede, consulte [controlador de rede](../../../sdn/technologies/network-controller/Network-Controller.md).

-   O Hyper-V pode hospedar os principais sistemas operacionais convidados de que você precisa.

## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualização de função de rede no Windows Server 2016

### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Funções de dispositivos virtuais fornecidas pela Microsoft
Os seguintes dispositivos virtuais são fornecidos com o Windows Server 2016:

**Balanceador de carga de software**

Um balanceador de carga de camada 4 operando na escala do datacenter. Esta é uma versão semelhante do balanceador de carga do Azure que foi implantada em escala no ambiente do Azure. Para obter mais informações sobre o Load Balancer de software da Microsoft, consulte [balanceamento de carga de software (SLB) para Sdn](/previous-versions/windows/server/mt632286(v=ws.12)). Para obter mais informações sobre Microsoft Azure serviços de balanceamento de carga, consulte [Microsoft Azure serviços de balanceamento de carga](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).

**Gateway**. O gateway RAS fornece todas as combinações das funções de gateway a seguir.

-   **Gateway site a site**

    O gateway de RAS fornece um gateway de multilocatário com capacidade de Border Gateway Protocol (BGP) que permite que seus locatários acessem e gerenciem seus recursos em conexões VPN site a site de sites remotos e que permite o fluxo de tráfego de rede entre os recursos virtuais nas redes físicas de locatário e nuvem. Para obter mais informações sobre o gateway de RAS, consulte gateway de [RAS de alta disponibilidade](/previous-versions/windows/server/mt631692(v=ws.12)) e [Gateway de Ras](../../../../remote/remote-access/ras-gateway/ras-gateway.md).

-   **Gateway de encaminhamento**

    O gateway RAS roteia o tráfego entre as redes virtuais e a rede física do provedor de hospedagem. Por exemplo, se os locatários criarem uma ou mais redes virtuais e precisarem de acesso a recursos compartilhados na rede física no provedor de hospedagem, o gateway de encaminhamento poderá rotear o tráfego entre a rede virtual e a rede física para fornecer aos usuários que trabalham na rede virtual com os serviços de que precisam. Para obter mais informações, consulte gateway de [RAS de alta disponibilidade](/previous-versions/windows/server/mt631692(v=ws.12)) e [Gateway de Ras](../../../../remote/remote-access/ras-gateway/ras-gateway.md).

-   **Gateways de túnel GRE**

    Os túneis baseados em GRE permitem a conectividade entre redes virtuais de locatário e redes externas. Como o protocolo GRE é leve e o suporte para o GRE está disponível na maioria dos dispositivos de rede, ele se torna uma opção ideal para encapsular onde a criptografia de dados não é necessária. O suporte a GRE em túneis Site a Site (S2S) resolve o problema de encaminhamento entre redes virtuais de locatário e redes externas de locatário usando um gateway multilocatário. Para obter mais informações sobre túneis GRE, consulte [túnel GRE no Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).

**Plano de controle de roteamento com BGP**

O controle de roteamento de HNV (virtualização de rede) do Hyper-V é a entidade lógica centralizada no plano de controle, que transporta todas as rotas de plano de endereço do cliente e aprende e atualiza dinamicamente os roteadores de gateway de RAS distribuídos na rede virtual. Para obter mais informações, consulte gateway de [RAS de alta disponibilidade](/previous-versions/windows/server/mt631692(v=ws.12)) e [Gateway de Ras](../../../../remote/remote-access/ras-gateway/ras-gateway.md).

**Firewall de vários locatários distribuído**

O firewall protege a camada de rede de redes virtuais. As políticas são impostas na porta SDN-vSwitch de cada VM de locatário. Ele protege todos os fluxos de tráfego: leste-oeste e norte-sul. As políticas são enviadas por push pelo portal de locatário e o controlador de rede as distribui para todos os hosts aplicáveis. Para obter mais informações sobre o firewall de vários locatários distribuído, consulte [visão geral do firewall do datacenter](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).