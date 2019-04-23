---
title: Visão geral do firewall do datacenter
description: Você pode usar este tópico para saber mais sobre o Firewall do Datacenter, que é uma camada de rede, 5 tuplas (números de porta de protocolo, origem e destino, endereços IP de origem e destino), firewall multilocatário, com monitoração de estado no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67576533-206b-428a-956c-ed8c53218d9b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f1de50dc61639f4985c9d28fdde6072af650f42e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890827"
---
# <a name="datacenter-firewall-overview"></a>Visão geral do firewall do datacenter

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Firewall do Datacenter é um novo serviço incluído com o Windows Server 2016. É uma camada de rede, 5 tuplas (protocolo, números de porta de origem e destino, endereços IP de origem e destino), firewall multilocatário, com monitoração de estado. Quando implantado e oferecido como um serviço pelo provedor de serviços, os administradores de locatários podem instalar e configurar políticas de firewall para ajudar a proteger suas redes virtuais contra tráfego indesejado proveniente da Internet e redes de intranet.  
  
![Firewall do Datacenter na pilha de rede](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
O administrador do provedor de serviço ou o administrador de locatários pode gerenciar as políticas de Firewall do Datacenter por meio de APIs northbound e o controlador de rede.  
  
O Firewall do Datacenter oferece as seguintes vantagens para os provedores de serviços de nuvem:  
  
-   Uma solução altamente escalonável, gerenciável e diagnosticáveis firewall baseado em software que pode ser oferecida aos locatários  
  
-   Liberdade para mover máquinas virtuais de locatário para hosts de computação diferentes sem interromper as políticas de firewall de locatário  
  
    -   Implantado como um firewall de agente de host de porta vSwitch  
  
    -   Máquinas virtuais de locatário obter as políticas atribuídas ao seu firewall do agente de host vSwitch  
  
    -   Regras de firewall são configuradas em cada porta vSwitch, independente do host real em execução na máquina virtual  
  
-   Oferece proteção para máquinas virtuais independentes do sistema operacional convidado locatário do locatário  
  
O Firewall do Datacenter oferece as seguintes vantagens para locatários:  
  
-   Capacidade de definir regras de firewall para ajudar a proteger as cargas de trabalho em redes virtuais de Internet  
  
-   Capacidade de definir regras de firewall para ajudar a proteger o tráfego entre máquinas virtuais na mesma L2 sub-rede virtual, bem como entre máquinas virtuais em diferentes sub-redes virtuais de L2  
  
-   Capacidade de definir regras de firewall para ajudar a proteger e isolar o tráfego de rede entre locatários redes e locais suas redes virtuais do provedor de serviços  
  


