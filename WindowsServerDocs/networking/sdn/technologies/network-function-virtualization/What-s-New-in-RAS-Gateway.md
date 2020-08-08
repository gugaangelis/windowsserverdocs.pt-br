---
title: Novidades no Gateway de RAS
description: Você pode usar este tópico para saber mais sobre os novos recursos do gateway de RAS, que é um roteador compatível com BGP (baseado em software, multilocatário Border Gateway Protocol) no Windows Server 2016.
manager: grcusanz
ms.topic: get-started-article
ms.assetid: 709cb192-313a-47b5-954e-eb5f6fee51a7
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 763764875b9213efdfbbef4e9ff1742a3734591f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969523"
---
# <a name="whats-new-in-ras-gateway"></a>Novidades no Gateway de RAS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre os novos recursos do gateway de RAS, que é um roteador compatível com BGP (baseado em software, multilocatário Border Gateway Protocol) no Windows Server 2016. O roteador BGP multilocatário do gateway de RAS foi projetado para CSPs (provedores de serviços de nuvem) e empresas que hospedam várias redes virtuais de locatário usando a virtualização de rede Hyper-V.

> [!NOTE]
> No Windows Server 2012 R2, o gateway RAS é chamado de gateway RRAS; e, no System Center Virtual Machine Manager, o gateway RAS é chamado de gateway do Windows Server.

Este tópico inclui as seções a seguir.

-   [Opções de conectividade site a site](#bkmk_s2s)

-   [Pools de gateway](#bkmk_pools)

-   [Escalabilidade do pool de gateway](#bkmk_gps)

-   [M + N redundância de pool de gateway](#bkmk_m)

-   [Refletor de rota](#bkmk_rr)

## <a name="site-to-site-connectivity-options"></a><a name="bkmk_s2s"></a>Opções de conectividade site a site
O gateway RAS agora dá suporte a três tipos de conexões VPN site a site: protocolo IKE versão 2 (IKEv2) site a site (VPN), VPN de camada 3 (L3) e túneis de encapsulamento de roteamento genérico (GRE).

Para obter mais informações sobre o GRE, consulte [túnel GRE no Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).

## <a name="gateway-pools"></a><a name="bkmk_pools"></a>Pools de gateway
No Windows Server 2016, você pode criar pools de gateway de tipos diferentes. Pools de gateway contêm muitas instâncias do gateway de RAS e roteiam o tráfego de rede entre redes físicas e virtuais. Os pools de gateway podem executar qualquer uma das funções de gateway individuais – protocolo IKE versão 2 (IKEv2) site a site (VPN), VPN de camada 3 (L3) e túneis de encapsulamento de roteamento genérico (GRE) – ou o pool pode executar todas essas funções e agir como um pool misto.

Você pode criar pools de gateway usando qualquer lógica preferida com base em seus requisitos de infraestrutura. Por exemplo, você pode criar pools de gateway com base em qualquer uma das características a seguir.

-   Tipos de túnel (VPN IKEv2, VPN L3, VPN GRE)

-   Capacity

-   Nível de redundância (confiabilidade baseada em seu plano de cobrança para locatários)

-   Separação personalizada para clientes

Para obter mais informações, consulte [alta disponibilidade do gateway de Ras](RAS-Gateway-High-Availability.md).

## <a name="gateway-pool-scalability"></a><a name="bkmk_gps"></a>Escalabilidade do pool de gateway
Você pode facilmente dimensionar um pool de gateway para cima ou para baixo adicionando ou removendo VMs de gateway no pool. A remoção ou a adição de gateways não interrompe os serviços fornecidos por um pool. Você também pode adicionar e remover pools inteiros de gateways.

Para obter mais informações, consulte [alta disponibilidade do gateway de Ras](RAS-Gateway-High-Availability.md).

## <a name="mn-gateway-pool-redundancy"></a><a name="bkmk_m"></a>M + N redundância de pool de gateway
Cada pool de gateway é M + N redundante. Isso significa que um ' M' número de VMs (máquinas virtuais) de gateway ativo é submetido a backup por um número ' N ' de VMs de gateway em espera. A redundância M + N fornece mais flexibilidade para determinar o nível de confiabilidade que você precisa ao implantar o gateway RAS. Em vez de usar apenas um gateway de RAS em espera por VM de gateway de RAS ativo – que é a única opção de configuração com o Windows Server 2012 R2, agora você pode configurar quantas VMs em espera você precisar. O recurso de Service Manager do gateway do controlador de rede usa com eficiência a capacidade da VM do gateway RAS em espera para fornecer um failover confiável se uma VM de gateway de RAS ativa falhar ou perder a conectividade.

Para obter mais informações, consulte [alta disponibilidade do gateway de Ras](RAS-Gateway-High-Availability.md).

## <a name="route-reflector"></a><a name="bkmk_rr"></a>Refletor de rota
O refletor de rota de Border Gateway Protocol (BGP) agora está incluído no gateway de RAS e fornece uma alternativa à topologia de malha completa do BGP que é necessária para a sincronização de rota entre roteadores. Com a sincronização de malha completa, todos os Roteadores BGP devem se conectar com todos os outros roteadores na topologia de roteamento. No entanto, quando você usa o refletor de rota, o refletor de rota é o único roteador que se conecta com todos os outros roteadores, chamados de clientes BGP, simplificando assim a sincronização de rota e reduzindo o tráfego de rede. O refletor de rota aprende todas as rotas, calcula as melhores rotas e redistribui as melhores rotas para seus clientes BGP.

Com o Windows Server 2016, você pode configurar os túneis de acesso remoto de um locatário para terminar em mais de uma VM de gateway de RAS. Isso fornece maior flexibilidade para provedores de serviço de nuvem quando você enfrenta circunstâncias em que uma VM de gateway RAS não pode atender a todos os requisitos de largura de banda das conexões de locatário.

Esse recurso, no entanto, apresenta a complexidade adicional do gerenciamento de rota e a sincronização efetiva de rotas entre os sites remotos de locatário e seus recursos virtuais no datacenter na nuvem. Fornecer locatários com conexões a vários gateways RAS também apresenta complexidade adicional na configuração no fim da empresa, em que cada site de locatário terá vizinhos de roteamento separados.

Um refletor de rota BGP no plano de controle resolve esses problemas e torna a implantação de malha interna do CSP transparente para os locatários empresariais. A seguir estão alguns pontos importantes sobre o refletor de rota BGP que está incluído no gateway de RAS e integrado com o controlador de rede.

-   Um refletor de rota em uma implantação de rede definida pelo software é uma entidade lógica que fica no plano de controle entre os gateways RAS e o controlador de rede. No entanto, ele não participa do roteamento do plano de dados.

-   Quando você adiciona um novo locatário ao seu datacenter, o controlador de rede configura automaticamente o primeiro gateway de RAS de locatário como um refletor de rota.

-   Cada locatário tem um refletor de rota correspondente e reside em uma das VMs de gateway de RAS que estão associadas a esse locatário.

-   Um refletor de rota de locatário atua como o refletor de rota para todas as VMs de gateway de RAS associadas ao locatário. Gateways de locatário diferentes do refletor de rota de gateway de RAS são os clientes de refletor de rota. O refletor de rota executa a sincronização de rota entre todos os clientes de refletor de rota para que o roteamento de caminho de dados real possa ocorrer.

-   Um refletor de rota não fornece serviços de refletor de rota para o gateway de RAS no qual ele está configurado.

-   Um refletor de rota atualiza o controlador de rede com as rotas empresariais que correspondem aos sites corporativos do locatário. Isso permite que o controlador de rede configure as políticas de virtualização de rede do Hyper-V necessárias na rede virtual de locatário para acesso de caminho de dados de ponta a ponta.

-   Se os clientes corporativos usam roteamento BGP no espaço de endereço do cliente, o refletor de rota do gateway RAS é o único vizinho BGP (eBGP) externo para todos os sites do locatário correspondente. Isso é verdadeiro independentemente dos pontos de encerramento de túnel do locatário da empresa. Em outras palavras, não importa qual VM de gateway de RAS no datacenter do CSP encerra o túnel VPN site a site para um site de locatário, o par eBGP para todos os sites de locatário é o refletor de rota.

Para obter mais informações, consulte [arquitetura de implantação do gateway de Ras](RAS-Gateway-Deployment-Architecture.md) e o tópico solicitação da IETF (Internet Engineering Task Force) para comentários [RFC 4456 BGP Route Reflection: uma alternativa para BGP (full mesh interno de malha)](https://tools.ietf.org/html/rfc4456).


