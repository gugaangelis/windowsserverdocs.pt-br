---
title: Novidades no Gateway de RAS
description: Você pode usar este tópico para saber mais sobre os novos recursos para o Gateway de RAS, que é uma baseada em software, multilocatário, o roteador capaz do Border Gateway Protocol (BGP) no Windows Server 2016.
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
ms.openlocfilehash: 5cc7d8bab3f2783750dbd723da745b1df3c2e462
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863017"
---
# <a name="whats-new-in-ras-gateway"></a>Novidades no Gateway de RAS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre os novos recursos para o Gateway de RAS, que é uma baseada em software, multilocatário, o roteador capaz do Border Gateway Protocol (BGP) no Windows Server 2016. O roteador BGP de multilocatário do Gateway de RAS foi projetado para provedores de serviço de nuvem (CSPs) e empresas que hospedam várias redes virtuais de locatário usando a virtualização de rede do Hyper-V.  
  
> [!NOTE]  
> No Windows Server 2012 R2, o Gateway de RAS é denominado Gateway RRAS; e no System Center Virtual Machine Manager, o Gateway de RAS é chamado o Gateway do Windows Server.  
  
Este tópico contém as seguintes seções.  
  
-   [Opções de conectividade de site a site](#bkmk_s2s)  
  
-   [Pools de gateway](#bkmk_pools)  
  
-   [Redimensionamento do Pool de gateway](#bkmk_gps)  
  
-   [Redundância do Pool de Gateway de M + N](#bkmk_m)  
  
-   [Refletor de rota](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>Opções de conectividade de site a site  
Gateway de RAS agora dá suporte a três tipos de conexões VPN site a site:  Protocolo IKE versão 2 (IKEv2) túneis de encapsulamento de roteamento genérico (GRE) de rede privada virtual (VPN) site a site e VPN de camada 3 (L3).  
  
Para obter mais informações sobre GRE, consulte [túnel GRE no Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="bkmk_pools"></a>Pools de gateway  
No Windows Server 2016, você pode criar pools de gateway de tipos diferentes. Pools de gateway contêm muitas instâncias do Gateway de RAS e rotear o tráfego de rede entre redes físicas e virtuais. Pools de gateway podem executar qualquer uma das funções individuais de gateway - protocolo IKE versão 2 (IKEv2) de túneis de encapsulamento de roteamento genérico (GRE) de rede privada virtual (VPN) site a site e VPN de camada 3 (L3) - ou o pool pode executar todas essas funções e atuar como um pool misto.  
  
Você pode criar pools de gateway usando qualquer lógica que você preferir com base em seus requisitos de infraestrutura. Por exemplo, você pode criar pools de gateway com base em qualquer uma das seguintes características.  
  
-   Tipos de túnel IKEv2 VPN, VPN L3, GRE VPN)  
  
-   Capacidade  
  
-   Nível de redundância (com base em seu plano de cobrança para locatários de confiabilidade)  
  
-   Separação personalizada para clientes  
  
Para obter mais informações, consulte [alta disponibilidade do Gateway de RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_gps"></a>Redimensionamento do Pool de gateway  
Você pode facilmente dimensionar um pool de gateway para cima ou para baixo, adicionando ou removendo as VMs de gateway no pool. Remoção ou adição de gateways não interrompa os serviços que são fornecidos por um pool. Você também pode adicionar e remover pools inteiros de gateways.  
  
Para obter mais informações, consulte [alta disponibilidade do Gateway de RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_m"></a>Redundância do Pool de Gateway de M + N  
Cada pool de gateway é com redundância M + N. Isso significa que um estou ' número de máquinas virtuais de gateway Active Directory (VMs) seja realizado por um número de ' n' de VMs de gateway em espera. M + N redundância fornece mais flexibilidade na determinação do nível de confiabilidade que você precisa, quando você implanta o Gateway de RAS. Em vez de usar apenas um Gateway de RAS em espera por active VM de Gateway de RAS, que é a única opção de configuração com o Windows Server 2012 R2 - agora você pode configurar várias VMs em espera forem necessárias. O recurso de Gerenciador de serviço de Gateway de controlador de rede com eficiência usa a capacidade da VM de Gateway de RAS em espera para fornecer failover confiável, se um ativo falhar ou perde a conectividade de VM de Gateway de RAS.  
  
Para obter mais informações, consulte [alta disponibilidade do Gateway de RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_rr"></a>Refletor de rota  
O refletor de rota do Border Gateway Protocol (BGP) agora está incluído com o Gateway de RAS e fornece uma alternativa para a topologia de malha completa de BGP é necessária para sincronização de rota entre roteadores. Com a sincronização de malha completa, todos os roteadores BGP devem se conectar com todos os outros roteadores na topologia de roteamento. No entanto, quando você usa o refletor de rota, o refletor de rota é o único roteador que se conecta com todos os outros roteadores, chamados de clientes via protocolo BGP, simplificando assim o roteiro de sincronização e reduzindo o tráfego de rede. O refletor de rota aprende todas as rotas, calcula melhores rotas e redistribui as rotas melhor para seus clientes BGP.  
  
Com o Windows Server 2016, você pode configurar túneis de acesso remoto de um locatário individual para terminar em mais de uma VM de Gateway de RAS. Isso fornece maior flexibilidade para provedores de serviço de nuvem ao se deparar com circunstâncias em que não é possível atender a uma VM de Gateway de RAS todos os requisitos de largura de banda das conexões de locatário.  
  
Essa funcionalidade, no entanto, apresenta complexidade adicional de gerenciamento de rota e sincronização efetiva de rotas entre os sites de locatário remoto e seus recursos virtuais no datacenter de nuvem. Também fornecer locatários as conexões para vários Gateways de RAS introduz uma complexidade adicional na configuração no final empresariais, onde cada site de locatário terá vizinhos roteamentos separados.  
  
Um refletor de rota BGP no plano de controle soluciona esses problemas e torna a implantação de malha interna de CSP transparente para os locatários da empresa. A seguir estão alguns pontos importantes sobre o refletor de rota BGP que é incluído com o Gateway de RAS e integrado ao controlador de rede.  
  
-   Um refletor de rota em uma implantação de rede definida pelo Software é uma entidade lógica que reside no plano de controle entre os Gateways de RAS e o controlador de rede. Ele não, no entanto, participa de roteamento de plano de dados.  
  
-   Quando você adiciona um novo locatário para o seu datacenter, o controlador de rede configura automaticamente o Gateway de RAS do primeiro locatário como um refletor de rota.  
  
-   Cada locatário tem um refletor de rota correspondente e ele reside em uma das VMs de Gateway de RAS que estão associados esse locatário.  
  
-   Um locatário refletor de rota atua como o refletor de rota para todas as VMs de Gateway de RAS que estão associadas ao locatário. Os gateways de locatário que não seja o refletor de rota do Gateway de RAS são os clientes de refletor de rota. O refletor de rota executa a sincronização de rota entre todos os clientes de refletor de rota para que o roteamento do caminho de dados reais possa ocorrer.  
  
-   Um refletor de rota não fornece serviços de refletor de rota para o Gateway de RAS no qual ele está configurado.  
  
-   Um refletor de rota atualiza o controlador de rede com as rotas de Enterprise que correspondem aos sites de empresa do locatário. Isso permite que o controlador de rede configurar as políticas de virtualização de rede do Hyper-V necessárias em uma rede virtual de locatário para acesso ao caminho de dados de ponta a ponta.  
  
-   Se seus clientes empresariais usarem roteamento de BGP no espaço de endereço do cliente, o refletor de rota do Gateway de RAS é o vizinho BGP (eBGP) apenas externo para todos os sites do locatário correspondente. Isso é verdadeiro independentemente de pontos de encerramento de túnel do locatário Enterprise. Em outras palavras, não importa qual VM de Gateway de RAS no CSP datacenter encerra o túnel VPN site a site para um site de locatário, o par de eBGP para todos os sites de locatário é o refletor de rota.  
  
Para obter mais informações, consulte [arquitetura de implantação do Gateway RAS](RAS-Gateway-Deployment-Architecture.md) e a solicitação da Internet Engineering Task Force (IETF) para o tópico de comentários [RFC 4456 BGP rota reflexão: Uma alternativa ao completo de malha BGP interno (IBGP)](https://tools.ietf.org/html/rfc4456).  
  

