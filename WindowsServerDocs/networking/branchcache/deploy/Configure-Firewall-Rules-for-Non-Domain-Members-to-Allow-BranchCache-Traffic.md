---
title: Configurar regras de firewall para membros não associados a domínio para permitir tráfego BranchCache
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 288865f0237969e0bed7e105f8d539759984275e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834767"
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>Configurar regras de firewall para membros não associados a domínio para permitir tráfego BranchCache

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar as informações neste tópico para configurar produtos de firewall de terceiros e configurar um computador cliente manualmente com regras de firewall que permitam a execução do BranchCache no modo de cache distribuído.  
  
> [!NOTE]  
> -   Se você configurou computadores cliente BranchCache usando a Diretiva de Grupo, as configurações da Diretiva de Grupo substituirão qualquer configuração manual de computadores cliente aos quais as diretivas foram aplicadas.  
> -   Se você implantou o BranchCache com DirectAccess, pode usar as configurações deste tópico para configurar regras de IPsec para permitir o tráfego de BranchCache.  
  
A associação a **Administradores** ou equivalente é o requisito mínimo para fazer essas alterações de configuração.  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD]: Protocolo de descoberta de recuperação e o cache de conteúdo de ponto a ponto  
Os clientes de cache distribuído devem permitir o tráfego MS-PCCRD de entrada e de saída executado no protocolo WS-Discovery.  
  
As configurações de firewall devem permitir o tráfego multicast, além do tráfego de entrada e de saída. Você pode usar as seguintes configurações para definir exceções de firewall para o modo de cache distribuído.  
  
Multicast IPv4: 239.255.255.250  
  
Multicast IPv6: FF02::C  
  
Tráfego de entrada: Porta local: 3702, porta remota: efêmero  
  
Tráfego de saída: Porta local: efêmero, porta remota: 3702  
  
Programa: %systemroot%\system32\svchost.exe (serviço de BranchCache [PeerDistSvc])  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR]: Recuperação e cache de conteúdo ponto a ponto: Protocolo de recuperação  
Os clientes de cache distribuído devem permitir o tráfego MS-PCCRR de entrada e de saída executado no protocolo HTTP 1.1 conforme documentado na RFC (solicitação de comentários) 2616.  
  
As configurações de firewall devem permitir o tráfego de entrada e de saída. Você pode usar as seguintes configurações para definir exceções de firewall para o modo de cache distribuído.  
  
Tráfego de entrada: Porta local: 80, porta remota: efêmero  
  
Tráfego de saída: Porta local: efêmero, porta remota: 80  
  


