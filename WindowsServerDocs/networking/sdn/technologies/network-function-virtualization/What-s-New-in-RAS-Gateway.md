---
title: O que há de novo no RAS Gateway
description: Você pode usar este tópico para saber mais sobre novos recursos RAS gateway, que é um baseada em software, multitenant, roteador capaz de Border Gateway Protocol (BGP) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 709cb192-313a-47b5-954e-eb5f6fee51a7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1a9ac762f6cd80d3889cf72478b7a7f8ce9e5cb7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ras-gateway"></a>O que há de novo no RAS Gateway

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre novos recursos RAS gateway, que é um baseada em software, multitenant, roteador capaz de Border Gateway Protocol (BGP) no Windows Server 2016. O roteador RAS Gateway Multitenant BGP se destina a provedores de serviço de nuvem (CSPs) e empresas que hospedam várias redes virtuais locatário usando a virtualização de rede do Hyper-V.  
  
> [!NOTE]  
> No Windows Server 2012 R2, o Gateway RAS é denominado Gateway de RRAS; e no System Center Virtual Machine Manager, Gateway RAS é chamada de Gateway de servidor do Windows.  
  
Este tópico contém as seções a seguir.  
  
-   [Opções de conectividade to-site](#bkmk_s2s)  
  
-   [Pools de gateway](#bkmk_pools)  
  
-   [Dimensionamento do Pool de gateway](#bkmk_gps)  
  
-   [M + N Gateway Pool redundância](#bkmk_m)  
  
-   [Refletor de rota](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>Opções de conectividade to-site  
Gateway RAS agora dá suporte a três tipos de conexões de-to-site VPN: Internet Key Exchange versão 2 (IKEv2) túneis-to-site rede virtual privada (VPN), a VPN Layer 3 (L3) e o encapsulamento de roteamento genérico (GRE).  
  
Para obter mais informações sobre GRE, consulte [GRE encapsulamento no Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="bkmk_pools"></a>Pools de gateway  
No Windows Server 2016, você pode criar grupos de gateway de tipos diferentes. Pools de gateway contêm muitas instâncias do Gateway RAS e rotear o tráfego de rede entre redes físicos e virtuais. Pools de gateway podem realizar qualquer uma das funções individuais gateway - Internet Key Exchange versão 2 (IKEv2)-to-site rede virtual privada (VPN), VPN Layer 3 (L3) e encapsulamento de roteamento genérico (GRE) túneis - ou o pool pode executar todas essas funções e atuar como um pool misto.  
  
Você pode criar grupos de gateway usando qualquer lógica que preferir com base nos seus requisitos de infraestrutura. Por exemplo, você pode criar pools de gateway com base em qualquer um dos seguintes características.  
  
-   Tipos de encapsulamento (IKEv2 VPN, L3 VPN, GRE VPN)  
  
-   Capacidade  
  
-   Nível de redundância (confiabilidade com base no seu plano de cobrança para locatários)  
  
-   Separação personalizada para os clientes  
  
Para obter mais informações, consulte [RAS Gateway alta disponibilidade](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_gps"></a>Dimensionamento do Pool de gateway  
Você pode dimensionar facilmente um pool de gateway para cima ou para baixo, adicionando ou removendo gateway VMs no pool. Remoção ou a adição de gateways não interromper os serviços que são fornecidos por um pool. Você também pode adicionar e remover todos pools de gateways.  
  
Para obter mais informações, consulte [RAS Gateway alta disponibilidade](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_m"></a>M + N Gateway Pool redundância  
Cada pool de gateway é M + N redundante. Isso significa que um estou ' diversas ativa gateway VMs (máquinas virtuais) são copiadas por um número de'n' de gateway espera VMs. M + N redundância fornece mais flexibilidade para determinar o nível de confiabilidade que exigem quando você implanta o Gateway RAS. Em vez de usar apenas um Gateway RAS espera por active VM do Gateway RAS - que é a única opção de configuração com o Windows Server 2012 R2 - agora você pode configurar VMs do modo de espera quantas forem necessárias. O recurso de Gerenciador de serviço de Gateway de controlador de rede com eficiência usa a capacidade de RAS Gateway VM no modo de espera para fornecer failover confiável se um ativo RAS Gateway VM falha ou perde conectividade.  
  
Para obter mais informações, consulte [RAS Gateway alta disponibilidade](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_rr"></a>Refletor de rota  
O refletor de rota Border Gateway Protocol (BGP) agora é incluído com o Gateway RAS e oferece uma alternativa a topologia em malha completa BGP que é necessária para a sincronização de rota entre roteadores. Com a sincronização de malha completo, todos os roteadores BGP devem se conectar com todos os outros roteadores a topologia de roteamento. Quando você usa refletor de rota, no entanto, o refletor de rota é o único roteador que se conecta com todos os outros roteadores, chamados BGP clientes, simplificando, assim, a sincronização de rota e reduzir o tráfego de rede. O refletor de rota aprende sobre todas as rotas, calcula melhores rotas e redistribui as rotas melhor a seus clientes BGP.  
  
Com o Windows Server 2016, você pode configurar túneis de acesso remoto do locatário um individuais de encerrar em mais de uma VM de Gateway RAS. Isso proporciona maior flexibilidade para provedores de serviço de nuvem ao se deparar com circunstâncias em que não pode atender a uma VM de Gateway RAS todos os requisitos de largura de banda de conexões de locatário.  
  
Essa funcionalidade, no entanto, apresenta a complexidade adicional de gerenciamento de rota e sincronização efetiva de rotas entre os locais remotos de locatário e seus recursos virtuais no datacenter na nuvem. Fornecendo locatários com conexões de vários Gateways RAS também introduz complexidade extra na configuração no final empresarial, onde cada site locatário terá vizinhos roteamento separados.  
  
Um refletor de rota BGP no plano controle lida com esses problemas e torna a implantação de malha interna do CSP transparente para os locatários empresarial. A seguir estão alguns pontos-chave sobre o refletor de rota BGP que é incluído com o Gateway RAS e integrado com o controlador de rede.  
  
-   A rota Reflector em uma implantação de Software de rede definidos é uma entidade lógica que fica no plano controle entre os Gateways RAS e o controlador de rede. Ele não, no entanto, participar na roteamento de plano de dados.  
  
-   Quando você adiciona um novo locatário para seu datacenter, o controlador de rede configura automaticamente o locatário primeiro Gateway RAS como um refletor de rota.  
  
-   Cada locatário tem um refletor de rota correspondente, e reside em um dos VMs do Gateway RAS associados esse locatário.  
  
-   Um locatário refletor de rota atua como o refletor de rota para todas as VMs do Gateway RAS associadas ao locatário. Gateways de locatário que não seja o refletor de rota de Gateway RAS são os clientes de refletor de rota. O refletor de rota realiza a sincronização de rota entre todos os clientes de refletor de rota para que o roteamento de caminho de dados reais pode ocorrer.  
  
-   A rota Reflector não fornecer serviços de refletor de rota para o Gateway RAS no qual ele está configurado.  
  
-   A rota Reflector atualiza o controlador de rede com as rotas empresarial que correspondem aos sites do Enterprise do locatário. Isso permite que o controlador de rede configurar as políticas de virtualização de rede do Hyper-V necessárias em uma rede virtual locatário para acesso a dados caminhos End-to-End.  
  
-   Se seus clientes empresariais usarem BGP roteamento no espaço de endereço do cliente, o refletor de rota de Gateway RAS é apenas externo vizinho BGP (eBGP) para todos os sites do locatário correspondente. Isso é verdadeiro, independentemente de pontos de encerramento de encapsulamento do locatário empresarial. Em outras palavras, não importa qual VM de Gateway RAS no CSP datacenter encerra o encapsulamento VPN-to-site para um site de locatário, o par eBGP para todos os sites de locatário é o refletor de rota.  
  
Para obter mais informações, consulte [arquitetura de implantação de Gateway RAS](RAS-Gateway-Deployment-Architecture.md) e a solicitação de Internet Engineering Task Force (IETF) para tópico comentários [RFC 4456 BGP rota reflexão: uma alternativa para completo de malha interna BGP (IBGP)](https://tools.ietf.org/html/rfc4456).  
  

