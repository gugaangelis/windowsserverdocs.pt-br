---
title: Túnel de GRE no Windows Server 2016
description: Você pode usar este tópico para obter uma compreensão das atualizações para o recurso de túnel de encapsulamento de roteamento genérico (GRE) para o gateway de RAS no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d246f0e56681f75e4336ed225d1557a0e05c581b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308552"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Túnel de GRE no Windows Server 2016

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

O Windows Server 2016 fornece atualizações para o encapsulamento de roteamento genérico \(recurso de túnel de\) GRE para o gateway RAS.  
  
GRE é um protocolo de túnel leve que pode encapsular uma ampla variedade de protocolos de camada de rede em links de ponto a ponto virtuais por meio de uma ligação entre redes de protocolo de Internet. A implementação do Microsoft GRE pode encapsular IPv4 e IPv6.  
  
Os túneis de GRE são úteis em muitos cenários porque:  
  
-   Eles são leves e compatíveis com RFC 2890, tornando-os interoperáveis com vários dispositivos de fornecedor  
  
-   Você pode usar Border Gateway Protocol \(BGP\) para roteamento dinâmico  
  
-   Você pode configurar gateways de RAS multilocatários do GRE para uso com rede definida pelo software \(SDN\)
  
-   Você pode usar System Center Virtual Machine Manager para gerenciar gateways de RAS baseados em\-de GRE
  
-   Você pode alcançar uma taxa de transferência de até 2,0 Gbps em uma máquina virtual de 6 núcleos configurada como um gateway de RAS do GRE
  
-   Um único gateway dá suporte A vários modos de conexão  
  
Os túneis baseados em GRE permitem a conectividade entre redes virtuais de locatário e redes externas. Como o protocolo GRE é leve e o suporte para GRE está disponível na maioria dos dispositivos de rede, ele se torna uma opção ideal para o túnel em que a criptografia de dados não é necessária. 

O suporte a GRE em túneis de S2S (site a site) resolve o problema de encaminhamento entre redes virtuais de locatário e redes externas de locatário usando um gateway de multilocatário, conforme descrito posteriormente neste tópico.  
  
O recurso de túnel GRE foi projetado para atender aos seguintes requisitos:  
  
-   Um provedor de hospedagem deve ser capaz de criar redes virtuais para encaminhamento sem modificar a configuração do comutador físico.  
  
-   Um provedor de hospedagem deve ser capaz de adicionar sub-redes a suas redes externas sem modificar a configuração dos comutadores físicos em sua infraestrutura.  
O recurso de túnel GRE habilita ou aprimora vários cenários principais para hospedar provedores de serviços usando tecnologias da Microsoft para implementar a rede definida pelo software em suas ofertas de serviço.  
  
Veja a seguir alguns cenários de exemplo:  
  
-   [Acesso de redes virtuais de locatário para redes físicas de locatário](#BKMK_Access)  
  
-   [Conectividade de alta velocidade](#BKMK_Speed)  
  
-   [Integração com isolamento baseado em VLAN](#BKMK_Integration)  
  
-   [Acessar recursos compartilhados](#BKMK_Shared)  
  
-   [Serviços de dispositivos de terceiros para locatários](#BKMK_thirdparty)  
  
## <a name="key-scenarios"></a>Principais cenários

A seguir estão os principais cenários aos quais o recurso de túnel GRE resolve.  
  
### <a name="access-from-tenant-virtual-networks-to-tenant-physical-networks"></a><a name="BKMK_Access"></a>Acesso de redes virtuais de locatário para redes físicas de locatário

Esse cenário permite uma maneira escalonável de fornecer acesso de redes virtuais de locatário para redes físicas de locatários localizadas no local do provedor de serviços de hospedagem. Um ponto de extremidade de túnel GRE é estabelecido no gateway multilocatário, o outro ponto de extremidade de túnel GRE é estabelecido em um dispositivo de terceiros na rede física. O tráfego de camada 3 é roteado entre as máquinas virtuais na rede virtual e o dispositivo de terceiros na rede física.  
  
![Túnel GRE conectando rede física do hoster e rede virtual de locatário](../../media/gre-tunneling-in-windows-server/GRE_.png)  
  
### <a name="high-speed-connectivity"></a><a name="BKMK_Speed"></a>Conectividade de alta velocidade

Esse cenário permite uma maneira escalonável de fornecer conectividade de alta velocidade da rede local do locatário para sua rede virtual localizada na rede do provedor de serviços de hospedagem. Um locatário se conecta à rede do provedor de serviços por meio da comutação de rótulo multiprotocolo (MPLS), em que um túnel GRE é estabelecido entre o roteador de borda do provedor de serviços de hospedagem e o gateway multilocatário para a rede virtual do locatário.  
  
![Túnel GRE conectando rede de locatário corporativo e rede virtual de locatário](../../media/gre-tunneling-in-windows-server/GRE-.png)  
  
### <a name="integration-with-vlan-based-isolation"></a><a name="BKMK_Integration"></a>Integração com isolamento baseado em VLAN

Esse cenário permite que você integre o isolamento baseado em VLAN com a virtualização de rede Hyper-V. Uma rede física na rede do provedor de hospedagem contém um balanceador de carga usando isolamento baseado em VLAN. Um gateway multilocatário estabelece túneis GRE entre o balanceador de carga na rede física e o gateway multilocatário na rede virtual.  
  
Vários túneis podem ser estabelecidos entre a origem e o destino, e a chave GRE é usada para discriminar várias entre os túneis.  
  
![Vários túneis GRE conectando redes virtuais de locatário](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)  
  
### <a name="access-shared-resources"></a><a name="BKMK_Shared"></a>Acessar recursos compartilhados

Esse cenário permite que você acesse recursos compartilhados em uma rede física localizada na rede do provedor de hospedagem.  
  
Você pode ter um serviço compartilhado localizado em um servidor em uma rede física localizada na rede do provedor de hospedagem que você deseja compartilhar com várias redes virtuais de locatário.  
  
As redes de locatários com sub-redes não sobrepostas acessam a rede comum em um túnel GRE. Um gateway de locatário único roteia entre os túneis GRE, portanto, roteando pacotes para as redes de locatário apropriadas.  
  
Nesse cenário, o gateway de locatário único pode ser substituído por dispositivos de hardware de terceiros.  
  
![Um gateway de locatário único usando vários túneis para conectar várias redes virtuais](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)  
  
### <a name="services-of-third-party-devices-to-tenants"></a><a name="BKMK_thirdparty"></a>Serviços de dispositivos de terceiros para locatários

Esse cenário pode ser usado para integrar dispositivos de terceiros (como balanceadores de carga de hardware) no fluxo de tráfego de rede virtual do locatário. Por exemplo, o tráfego originado de um site corporativo passa por um túnel S2S para o gateway multilocatário. O tráfego é roteado para o balanceador de carga em um túnel GRE. O balanceador de carga roteia o tráfego para várias máquinas virtuais na rede virtual da empresa. A mesma coisa acontece para outro locatário com endereços IP potencialmente sobrepostos nas redes virtuais. O tráfego de rede é isolado no balanceador de carga usando VLANs e é aplicável a todos os dispositivos de camada 3 que dão suporte a VLANs.  
  
![Vários túneis GRE conectando redes virtuais a dispositivos de terceiros](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)  
  
## <a name="configuration-and-deployment"></a>Configuração e implantação

Um túnel GRE é exposto como um protocolo adicional em uma interface S2S. Ele é implementado de maneira semelhante como um túnel S2S IPSec descrito no seguinte blog de rede: [Gateway de VPN site a site (S2S) multilocatário com o Windows Server 2012 R2](https://blogs.technet.com/b/networking/archive/2013/09/29/multi-tenant-site-to-site-s2s-vpn-gateway-with-windows-server-2012-r2.aspx)  
  
Consulte o tópico a seguir para obter um exemplo que implanta gateways, incluindo gateways de túnel GRE:  
  
[Implantar uma infraestrutura de rede definida pelo software usando scripts](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
  
## <a name="more-information"></a>Mais informações

Para obter mais informações sobre como implantar gateways S2S, consulte os seguintes tópicos:  
  
-   [Gateway de RAS](RAS-Gateway.md)  
  
-   [Border Gateway Protocol &#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)  
  
-   [Novo! Guia de implantação do gateway multilocatário do Windows Server 2012 R2 RAS](https://blogs.technet.com/b/wsnetdoc/archive/2014/03/26/new-windows-server-2012-r2-RAS-multitenant-gateway-deployment-guide.aspx)  
  
-   [Implantar Border Gateway Protocol (BGP) com o gateway de multilocatário do RAS](https://blogs.technet.com/b/wsnetdoc/archive/2014/04/03/deploy-border-gateway-protocol-bgp-with-the-RAS-multitenant-gateway.aspx)  
  


