---
title: Virtualização de função de rede
description: Você pode usar este tópico para saber mais sobre virtualização de função de rede, que permite que você implante soluções de virtualização de rede, como o Firewall do Datacenter, Gateway de RAS multilocatário e Software SLB balanceamento de carga () no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 59474a13d1cbce6a607f025caf3f6c1b839c7eed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884547"
---
# <a name="network-function-virtualization"></a>Virtualização de função de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre virtualização de função de rede, que permite que você implante soluções de virtualização de rede, como o Firewall do Datacenter, multilocatário Gateway RAS e balanceamento de carga de Software \(SLB\) multiplexador \(MUX\).
  
>[!NOTE]  
>Além deste tópico, a seguinte documentação de virtualização de função de rede está disponível.  
> - [Visão geral do Firewall do Datacenter](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [Gateway RAS para SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [Software balanceamento de carga (SLB) para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
No software de hoje datacenters definidos, as funções de rede que estão sendo executadas por dispositivos de hardware (como balanceadores de carga, firewalls, roteadores, comutadores e assim por diante) são cada vez mais sendo virtualizadas como soluções de virtualização. Essa "virtualização da função de rede" é uma progressão natural de virtualização do servidor e de virtualização de rede. Soluções de virtualização são emergentes e criando um novo mercado rapidamente. Eles continuam a gerar interesse e obter um impulso em ambas as plataformas de virtualização e serviços de nuvem.  
  
A Microsoft incluiu um autônomo gateway como um dispositivo virtual a partir do Windows Server 2012 R2. Para obter mais informações, consulte [Gateway do Windows Server](https://technet.microsoft.com/library/dn313101.aspx). Agora com o Windows Server 2016 a Microsoft continua a expandir e investir no mercado de virtualização de função de rede.  
  
## <a name="virtual-appliance-benefits"></a>Benefícios da solução de virtualização  
Um dispositivo virtual é dinâmico e fácil de alterar porque ele é uma máquina virtual predefinida e personalizada. Ele pode ser um ou mais máquinas virtuais empacotados, atualizado e mantido como uma unidade. (SDN) de rede definida pelo junto com o software, você obterá a agilidade e a flexibilidade necessária na infraestrutura baseada em nuvem de hoje. Por exemplo:  
  
-   SDN apresenta a rede como um recurso dinâmico e em pool.  
  
-   SDN facilita o isolamento de locatários.  
  
-   SDN maximiza o desempenho e escala.  
  
-   Soluções de virtualização de habilitar a mobilidade perfeita capacidade de expansão e a carga de trabalho.  
  
-   Soluções de virtualização de minimizar a complexidade operacional.  
  
-   Dispositivos virtuais permitem que os clientes adquirir, implantar e gerenciar soluções previamente integradas facilmente.  
  
    -   Os clientes podem mover facilmente a solução de virtualização em qualquer lugar na nuvem.  
  
    -   Os clientes soluções de virtualização podem ser dimensionado para cima ou para baixo dinamicamente com base na demanda.  
  
Para obter mais informações sobre o Microsoft SDN, consulte [rede definida pelo Software](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-).  
  
### <a name="what-network-functions-are-being-virtualized"></a>Quais funções de rede estão sendo virtualizadas?  
O marketplace para funções de rede virtualizada está crescendo rapidamente. As seguintes funções de rede estão sendo virtualizadas:  
  
-   **Segurança**  
  
    -   Firewall  
  
    -   Antivírus  
  
    -   DDoS (negação de serviço em distribuído)  
  
    -   IPS/IDS (sistema de detecção de intrusão/sistema de prevenção contra intrusão)  
  
-   **Otimizadores WAN/aplicativo**  
  
-   **Edge**  
  
    -   Gateway site a site  
  
    -   Gateways de L3  
  
    -   Roteadores  
  
    -   Comutadores  
  
    -   NAT  
  
    -   Balanceadores de carga (não necessariamente na borda)  
  
    -   Proxy HTTP  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Por que a Microsoft é uma excelente plataforma para soluções de virtualização  
![Pilha de rede virtual](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
A plataforma da Microsoft foi projetada para ser uma excelente plataforma para compilar e implantar soluções de virtualização. Eis o porquê:  
  
-   A Microsoft fornece funções de chave virtualizada de rede com o Windows Server 2016.  
  
-   Você pode implantar uma solução de virtualização do fornecedor de sua escolha.  
  
-   Você pode implantar, configurar e gerenciar suas soluções de virtualização com o controlador de rede da Microsoft que acompanha o Windows Server 2016. Para obter mais informações sobre o controlador de rede, consulte [controlador de rede](../../../sdn/technologies/network-controller/Network-Controller.md).  
  
-   Hyper-V pode hospedar os sistemas operacionais convidados principais que você precisa.  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualização de função de rede no Windows Server 2016  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Funções de dispositivos virtuais fornecidas pela Microsoft  
As soluções de virtualização a seguir são fornecidas com o Windows Server 2016:  
  
**Balanceador de carga de software**  
  
Um balanceador de carga de camada 4 operar em grande escala do datacenter. Esta é uma versão semelhante do balanceador de carga do Azure que tenha sido implantado em grande escala no ambiente do Azure. Para obter mais informações sobre o balanceador de carga de Software da Microsoft, consulte [Software SLB balanceamento de carga () para SDN](https://technet.microsoft.com/library/mt632286.aspx). Para obter mais informações sobre os serviços de balanceamento de carga do Microsoft Azure, consulte [serviços de balanceamento de carga do Microsoft Azure](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).  
  
**Gateway**. O Gateway de RAS fornece todas as combinações das seguintes funções do gateway.  
  
-   **Gateway site a Site**  
  
    O Gateway de RAS fornece um Border Gateway Protocol (BGP)-gateway de multilocatário, com capacidade que permite que seus locatários acessar e gerenciar seus recursos em conexões de VPN site a site de sites remotos e que permite que o fluxo do tráfego de rede entre recursos virtuais na nuvem e locatário redes físicas. Para obter mais informações sobre o Gateway de RAS, consulte [alta disponibilidade do Gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) e [Gateway RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Gateway de encaminhamento**  
  
    Gateway de RAS roteia o tráfego entre redes virtuais e a rede física provedor de hospedagem. Por exemplo, se locatários criar uma ou mais redes virtuais e precisam de acesso aos recursos compartilhados na rede física no provedor de hospedagem, o gateway de encaminhamento pode rotear o tráfego entre a rede virtual e a rede física para fornecer aos usuários trabalhando em a rede virtual com os serviços que eles precisam. Para obter mais informações, consulte [alta disponibilidade do Gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) e [Gateway RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Gateways de túnel GRE**  
  
    Os túneis baseados em GRE permitem a conectividade entre redes virtuais de locatário e redes externas. Uma vez que o protocolo GRE é leve e o suporte para GRE está disponível na maioria dos dispositivos de rede, ele se torna uma opção ideal para túnel quando a criptografia de dados não é necessária. O suporte a GRE em túneis Site a Site (S2S) resolve o problema de encaminhamento entre redes virtuais de locatário e redes externas de locatário usando um gateway multilocatário. Para obter mais informações sobre túneis GRE, consulte [túnel GRE no Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx).  
  
**Plano de controle de roteamento com BGP**  
  
Controle de roteamento de virtualização de rede do Hyper-V (HNV) é a entidade lógica e centralizada no plano de controle, que executa todas as rotas de plano de endereço do cliente e aprende dinamicamente e, em seguida, atualiza os roteadores de Gateway de RAS distribuídos na rede virtual. Para obter mais informações, consulte [alta disponibilidade do Gateway RAS](https://technet.microsoft.com/library/mt631692.aspx) e [Gateway RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
**Firewall de multilocatário distribuído**  
  
O firewall protege a camada de rede de redes virtuais. As políticas são impostas na porta de SDN vSwitch de cada VM de locatário. Ele protege todos os fluxos de tráfego: Oeste Leste e Norte-Sul. As políticas são enviados por push por meio do portal de locatário e o controlador de rede distribui-los a todos os hosts aplicáveis. Para obter mais informações sobre o firewall multilocatário distribuído, consulte [visão geral do Firewall do Datacenter](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  


