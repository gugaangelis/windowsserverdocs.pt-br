---
title: Túnel de GRE no Windows Server 2016
description: Você pode usar este tópico para obter uma compreensão das atualizações para o recurso de encapsulamento de encapsulamento de roteamento genérico (GRE) para o Gateway de RAS no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0ec077ad5e97edd3db7d1dc4e662bb191f7885b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887857"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Túnel de GRE no Windows Server 2016

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Windows Server 2016 fornece atualizações para o encapsulamento de roteamento genérico \(GRE\) capacidade de criar um túnel para o Gateway de RAS.  
  
GRE é um protocolo de túnel leve que pode encapsular uma ampla variedade de protocolos de camada de rede em links de ponto a ponto virtuais por meio de uma ligação entre redes de protocolo de Internet. A implementação da Microsoft GRE pode encapsular IPv4 e IPv6.  
  
Túneis GRE são úteis em muitos cenários porque:  
  
-   Eles são leves e 2890 RFC em conformidade, tornando-se interoperável com vários dispositivos de fornecedor  
  
-   Você pode usar o Border Gateway Protocol \(BGP\) para roteamento dinâmico  
  
-   Você pode configurar Gateways de RAS de multilocatário GRE para uso com a rede definida pelo Software \(SDN\)
  
-   Você pode usar o System Center Virtual Machine Manager para gerenciar o GRE\-com base em Gateways de RAS
  
-   Você pode obter uma taxa de transferência de até 2,0 Gbps em uma máquina virtual de 6 núcleos que esteja configurada como um Gateway de RAS do GRE
  
-   Um único gateway dá suporte a vários modos de conexão  
  
Os túneis baseados em GRE permitem a conectividade entre redes virtuais de locatário e redes externas. Uma vez que o protocolo GRE é leve e o suporte para GRE está disponível na maioria dos dispositivos de rede se torna uma opção ideal para túnel quando a criptografia de dados não é necessária. 

Suporte a GRE em túneis do Site a Site (S2S) resolve o problema de encaminhamento entre redes virtuais de locatário e redes externas de locatário usando um gateway de multilocatário, conforme descrito mais adiante neste tópico.  
  
O recurso de túnel GRE é projetado para atender os seguintes requisitos:  
  
-   Um provedor de hospedagem deve ser capaz de criar redes virtuais para encaminhamento sem modificar a configuração de comutador físico.  
  
-   Um provedor de hospedagem deve ser capaz de adicionar sub-redes a suas redes voltado para o exterior sem modificar a configuração dos comutadores físicos dentro de sua infra-estrutura.  
O recurso de túnel GRE permite ou aprimora a vários cenários de chave para provedores de serviço usando as tecnologias da Microsoft para implementar a rede definida pelo Software em suas ofertas de serviço de hospedagem.  
  
A seguir estão alguns cenários de exemplo:  
  
-   [Acesso de redes virtuais de locatário para redes físicas de locatário](#BKMK_Access)  
  
-   [Conectividade de alta velocidade](#BKMK_Speed)  
  
-   [Integração com o isolamento com base em VLAN](#BKMK_Integration)  
  
-   [Recursos de acesso compartilhado](#BKMK_Shared)  
  
-   [Serviços de dispositivos de terceiros para locatários](#BKMK_thirdparty)  
  
## <a name="key-scenarios"></a>Principais cenários

A seguir estão os principais cenários que endereços de recurso de túnel de GRE.  
  
### <a name="BKMK_Access"></a>Acesso de redes virtuais de locatário para redes físicas de locatário

Esse cenário permite que uma maneira escalonável fornecer acesso de redes virtuais de locatário para redes físicas localizadas no local de provedor de serviço hospedagem de locatário. Um ponto de extremidade do túnel GRE é estabelecido no gateway de multilocatário, o outro ponto de extremidade de túnel GRE é estabelecido em um dispositivo de terceiros na rede física. Tráfego de camada 3 é roteado entre as máquinas virtuais na rede virtual e o dispositivo de terceiros na rede física.  
  
![Conexão de rede física do hoster e rede virtual do locatário de túnel GRE](../../media/gre-tunneling-in-windows-server/GRE_.png)  
  
### <a name="BKMK_Speed"></a>Conectividade de alta velocidade

Esse cenário permite que uma maneira escalonável fornecer conectividade de alta velocidade da rede de local do locatário para sua rede virtual localizada na rede de provedor de serviço de hospedagem. Um locatário se conecta à rede de provedor de serviço por meio de mudança (MPLS), em que um túnel GRE é estabelecido entre o gateway de multilocatário para rede virtual do locatário e o roteador de borda do provedor de serviço hospedagem de rótulo multiprotocolo.  
  
![Conexão de rede do locatário empresarial MPLS e rede virtual do locatário de túnel GRE](../../media/gre-tunneling-in-windows-server/GRE-.png)  
  
### <a name="BKMK_Integration"></a>Integração com o isolamento com base em VLAN

Esse cenário permite que você integre o isolamento de VLAN com base em com virtualização de rede do Hyper-V. Uma rede física em rede do provedor de hospedagem contém um balanceador de carga usando o isolamento de VLAN-based. Um gateway multilocatário estabelece túneis GRE entre o balanceador de carga na rede física e o gateway multilocatário em uma rede virtual.  
  
Vários túneis podem ser estabelecidos entre a origem e destino, e a chave GRE é usada para Discriminar entre os túneis.  
  
![Conectar redes virtuais de locatário de túneis GRE vários](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)  
  
### <a name="BKMK_Shared"></a>Recursos de acesso compartilhado

Esse cenário permite que você acesse recursos compartilhados em uma rede física localizado na rede de provedor de hospedagem.  
  
Você pode ter um serviço compartilhado, localizado em um servidor em uma rede física localizado na rede de provedor de hospedagem que você deseja compartilhar com várias redes virtuais de locatário.  
  
As redes de locatários com sub-redes não sobrepostos acessam a rede comum por um túnel GRE. Um gateway de único locatário rotas entre os túneis GRE, portanto, roteamento de pacotes para as redes de locatário apropriada.  
  
Nesse cenário, o gateway de único locatário pode ser substituído por dispositivos de hardware de terceiros.  
  
![Um gateway de único locatário usando vários túneis para se conectar a várias redes virtuais](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)  
  
### <a name="BKMK_thirdparty"></a>Serviços de dispositivos de terceiros para locatários

Esse cenário pode ser usado para integrar os dispositivos de terceiros (por exemplo, balanceadores de carga de hardware) para o fluxo de tráfego de rede virtual do locatário. Por exemplo, o tráfego originado de um site corporativo passa por um túnel de S2S para o gateway multilocatário. O tráfego é roteado para o balanceador de carga por um túnel GRE. O balanceador de carga roteia o tráfego para várias máquinas virtuais na rede virtual da empresa. A mesma coisa acontece para outro locatário com potencialmente a sobreposição de endereços IP nas redes virtuais. O tráfego de rede é isolado no balanceador de carga usando VLANs e é aplicável a todos os dispositivos de camada 3 que dão suporte a VLANs.  
  
![Conexão de redes virtuais a dispositivos de terceiros de GRE vários túneis](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)  
  
## <a name="configuration-and-deployment"></a>Configuração e implantação

Um túnel GRE é exposto como um protocolo adicional dentro de uma interface de S2S. Ele é implementado de maneira semelhante como um túnel IPSec S2S descrito no seguinte Blog do serviço de rede: [Gateway VPN multilocatário-to-Site (S2S) com o Windows Server 2012 R2](https://blogs.technet.com/b/networking/archive/2013/09/29/multi-tenant-site-to-site-s2s-vpn-gateway-with-windows-server-2012-r2.aspx)  
  
Consulte o tópico a seguir para obter um exemplo que implanta gateways, incluindo os gateways de túnel GRE:  
  
[Implantar uma infraestrutura de rede definida por Software usando scripts](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
  
## <a name="more-information"></a>Mais informações

Para obter mais informações sobre como implantar gateways S2S, consulte os tópicos a seguir:  
  
-   [Gateway de RAS](RAS-Gateway.md)  
  
-   [Border Gateway Protocol &#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)  
  
-   [Novo! Guia de implantação do Gateway multilocatário de RAS do Windows Server 2012 R2](https://blogs.technet.com/b/wsnetdoc/archive/2014/03/26/new-windows-server-2012-r2-RAS-multitenant-gateway-deployment-guide.aspx)  
  
-   [Implantar o Border Gateway Protocol (BGP) com o Gateway multilocatário do RAS](https://blogs.technet.com/b/wsnetdoc/archive/2014/04/03/deploy-border-gateway-protocol-bgp-with-the-RAS-multitenant-gateway.aspx)  
  


