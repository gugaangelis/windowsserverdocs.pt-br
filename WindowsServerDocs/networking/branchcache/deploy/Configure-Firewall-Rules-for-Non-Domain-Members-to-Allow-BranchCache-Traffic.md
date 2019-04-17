---
title: Configurar as regras de Firewall para membros do domínio não permitir o tráfego BranchCache
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e5f744141efc35bb493bcd95fad53eafbbc3f78d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>Configurar as regras de Firewall para membros do domínio não permitir o tráfego BranchCache

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar as informações neste tópico para configurar produtos de firewall de terceiros e configurar manualmente um computador cliente com regras de firewall que permitam BranchCache executar no modo de cache distribuído.  
  
> [!NOTE]  
> -   Se você tiver configurado BranchCache os computadores cliente usando a política de grupo, as configurações de política de grupo substituem qualquer configuração manual de computadores cliente para que as políticas são aplicadas.  
> -   Se você implantou BranchCache com DirectAccess, você pode usar as configurações neste tópico para configurar as regras de IPsec para permitir o tráfego BranchCache.  
  
A associação ao grupo **administradores**, ou equivalente é o requisito mínimo para fazer essas alterações de configuração.  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD]: par cache de conteúdo e protocolo de descoberta de recuperação  
Os clientes de cache distribuído devem permitir o tráfego de MS-PCCRD de entrada e saída, que é executado no protocolo detecção dinâmica de serviços de Web (WS-Discovery).  
  
Configurações do firewall devem permitir o tráfego multicast além de tráfego de entrada e saído. Você pode usar as seguintes configurações para configurar as exceções de firewall para o modo de cache distribuído.  
  
Multicast IPv4: 239.255.255.250  
  
Multicast IPv6: FF02::C  
  
O tráfego de entrada: porta Local: 3702, porta remota: efêmero  
  
O tráfego de saída: porta Local: porta remota efêmera: 3702  
  
Programa: %systemroot%\system32\svchost.exe (serviço de BranchCache [PeerDistSvc])  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR]: par cache de conteúdo e recuperação: protocolo de recuperação  
Os clientes de cache distribuído devem permitir o tráfego de MS-PCCRR de entrada e saída, que é executado no protocolo HTTP 1.1 conforme documentado na solicitação de comentários (RFC) 2616.  
  
Configurações do firewall devem permitir o tráfego de entrada e saído. Você pode usar as seguintes configurações para configurar as exceções de firewall para o modo de cache distribuído.  
  
O tráfego de entrada: porta Local: 80, porta remota: efêmero  
  
O tráfego de saída: porta Local: porta remota efêmera: 80  
  


