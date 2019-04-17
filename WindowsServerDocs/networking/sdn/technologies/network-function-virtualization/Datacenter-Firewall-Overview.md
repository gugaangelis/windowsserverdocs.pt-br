---
title: Visão geral sobre Datacenter Firewall
description: Você pode usar este tópico para saber mais sobre Datacenter Firewall, que é uma camada de rede, 5 tuplas (números de porta de protocolo, de origem e de destino, endereços IP de origem e de destino), firewall com monitoração de estado, vários locatários no Windows Server 2016.
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
ms.openlocfilehash: 0c9b9fb5b0fb9aa09ed783b2b66a8ad370a627c3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="datacenter-firewall-overview"></a>Visão geral sobre Datacenter Firewall

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Datacenter Firewall é um novo serviço incluído no Windows Server 2016. É uma camada de rede, 5 tuplas (protocolo, os números de porta de origem e de destino, endereços IP de origem e de destino), firewall com monitoração de estado, vários locatários. Quando implantado e oferecido como um serviço pelo provedor de serviço, os administradores locatários podem instalar e configurar as políticas de firewall para ajudar a proteger suas redes virtuais de indesejado tráfego originado da Internet e redes de intranet.  
  
![Datacenter Firewall na pilha de rede](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
O administrador do provedor de serviço ou o administrador de locatário pode gerenciar as políticas de Firewall de Datacenter via o controlador de rede e as APIs em sentido norte.  
  
O Firewall de Datacenter oferece as seguintes vantagens para provedores de serviço de nuvem:  
  
-   Uma solução de firewall altamente escalável, gerenciável e diagnosticável baseada em software que pode ser oferecida a locatários  
  
-   Liberdade para mover máquinas virtuais de locatário para computação diferentes hosts sem quebrar políticas de firewall de locatário  
  
    -   Implantado como um firewall de agente de host de porta vSwitch  
  
    -   Máquinas virtuais de locatário obter as políticas atribuídas ao seu firewall de agente de host vSwitch  
  
    -   As regras de firewall são configuradas em cada porta vSwitch, independente do host real executando a máquina virtual  
  
-   Oferece proteção para máquinas virtuais independentes do sistema operacional convidado locatário do locatário  
  
O Firewall de Datacenter oferece as seguintes vantagens para locatários:  
  
-   Capacidade de definir as regras de firewall para ajudar a proteger com cargas de trabalho em redes virtuais a Internet  
  
-   Capacidade de definir as regras de firewall para ajudar a proteger o tráfego entre máquinas virtuais na mesma L2 virtual sub-rede, bem como entre máquinas virtuais em várias sub-redes virtuais L2  
  
-   Capacidade de definir regras de firewall para ajudar a proteger e isolar o tráfego de rede entre locatário local redes e suas redes virtuais do provedor de serviços  
  


