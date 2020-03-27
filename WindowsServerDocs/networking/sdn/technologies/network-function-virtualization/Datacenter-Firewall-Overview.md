---
title: Visão geral do firewall do datacenter
description: Você pode usar este tópico para saber mais sobre o Firewall do datacenter, que é uma camada de rede, 5 tuplas (protocolo, números de porta de origem e de destino, endereços IP de origem e de destino), firewall multilocatário com estado no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67576533-206b-428a-956c-ed8c53218d9b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 141057d2ee3e648f589d255ea04fdef179cedf3c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317030"
---
# <a name="datacenter-firewall-overview"></a>Visão geral do firewall do datacenter

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

O Firewall do datacenter é um novo serviço incluído no Windows Server 2016. É uma camada de rede, de 5 tuplas (protocolo, números de porta de origem e de destino, endereços IP de origem e de destino), firewall com estado e multilocatário. Quando implantado e oferecido como um serviço pelo provedor de serviços, os administradores de locatários podem instalar e configurar políticas de firewall para ajudar a proteger suas redes virtuais contra tráfego indesejado provenientes de redes de Internet e intranet.  
  
![Firewall do datacenter na pilha de rede](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
O administrador do provedor de serviços ou o administrador de locatários pode gerenciar as políticas de firewall do datacenter por meio do controlador de rede e das APIs do northbound.  
  
O Firewall do datacenter oferece as seguintes vantagens para provedores de serviços de nuvem:  
  
-   Uma solução de firewall baseada em software altamente escalonável, gerenciável e diagnosticada que pode ser oferecida aos locatários  
  
-   Liberdade para mover máquinas virtuais de locatário para diferentes hosts de computação sem interromper as políticas de firewall de locatário  
  
    -   Implantado como um firewall de agente de host de porta vSwitch  
  
    -   As máquinas virtuais de locatário obtêm as políticas atribuídas ao firewall do agente de host vSwitch  
  
    -   As regras de firewall são configuradas em cada porta vSwitch, independentemente do host real que executa a máquina virtual  
  
-   Oferece proteção para máquinas virtuais de locatário independentemente do sistema operacional convidado de locatário  
  
O Firewall do datacenter oferece as seguintes vantagens para os locatários:  
  
-   Capacidade de definir regras de firewall para ajudar a proteger cargas de trabalho voltadas para a Internet em redes virtuais  
  
-   Capacidade de definir regras de firewall para ajudar a proteger o tráfego entre máquinas virtuais na mesma sub-rede virtual de L2, bem como entre máquinas virtuais em sub-redes virtuais de L2 diferentes  
  
-   Capacidade de definir regras de firewall para ajudar a proteger e isolar o tráfego de rede entre redes locais de locatário e suas redes virtuais no provedor de serviços  
  


