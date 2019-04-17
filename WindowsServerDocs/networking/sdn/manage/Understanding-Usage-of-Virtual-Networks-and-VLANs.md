---
title: Noções básicas sobre o uso de redes virtuais e VLANs
description: Este tópico faz parte do Software de rede definidos guia sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fcf84c5c1f0be2fa1c7524592f8d02e4b11d3a2b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="understanding-usage-of-virtual-networks-and-vlans"></a>Noções básicas sobre o uso de redes virtuais e VLANs

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre redes virtuais de virtualização do Hyper-V rede e como eles diferem dos redes locais virtuais (VLANs).  
  
Software definido de rede (SDN) no Windows Server 2016 baseia-se em programação política para redes virtuais sobreposição dentro de um comutador Virtual Hyper-V. Você pode criar redes virtuais de sobreposição, também chamadas de redes virtuais, com a virtualização de rede do Hyper-V.   
  
Quando você implanta virtualização de rede do Hyper-V, redes de sobreposição são criados, encapsulando quadro de Ethernet de camada 2 o original locatário da máquina virtual com uma sobreposição - ou túnel - cabeçalho (por exemplo, VXLAN ou NVGRE) e a rede de cabeçalhos de base (ou física) Layer 3 IP e Ethernet de camada 2. As redes virtuais de sobreposição são identificadas por um 24 bits Virtual rede identificador (VNI) para manter o isolamento do locatário tráfego e permitir que endereços IP sobrepostos. O VNI é composto por uma sub-rede virtual ID (VSID), ID do switch lógico e ID de encapsulamento.  
  
Além disso, cada locatário é atribuído a um domínio de roteamento (semelhante de roteamento virtual e encaminhamento - VRF) para que vários prefixos de sub-rede virtual (cada uma representado por um VNI) podem ser roteados diretamente uns aos outros. Entre locatário (ou entre domínios de roteamento) não há suporte para roteamento sem passar por um gateway.   
  
A rede física no qual tráfego encapsulado do cada locatário é encapsulado é representada por uma rede lógica chamada rede lógico do provedor. Essa rede lógico do provedor consiste em uma ou mais sub-redes, cada uma representado por um prefixo de IP e, opcionalmente, uma VLAN 802.1 q marca.  
  
Você pode criar redes lógicas adicionais e sub-redes para fins de infraestrutura realizar o tráfego de gerenciamento, o tráfego de armazenamento, live tráfego da migração, etc.  
  
Microsoft SDN não suporta o isolamento das redes de locatário usando VLANs. Isolamento de locatário é feito exclusivamente usando a sobreposição de virtualização de rede do Hyper-V redes virtuais e encapsulamento. 


