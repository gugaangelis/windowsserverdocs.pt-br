---
title: Entender o uso de redes virtuais e VLANs
description: Neste tópico, você aprende sobre as redes virtuais de virtualização de rede Hyper-V e como elas diferem das VLANs (redes locais virtuais). Com a virtualização de rede Hyper-V, você cria sobreposição de redes virtuais, também chamadas de redes virtuais.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84ac2458-3fcf-4c4f-acfe-6105443dd83f
ms.author: pashort
author: shortpatti
ms.date: 08/26/2018
ms.openlocfilehash: 854adf0e7bb2a8715e3d447c04e2f09c3470a781
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355828"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>Entender o uso de redes virtuais e VLANs

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Neste tópico, você aprende sobre as redes virtuais de virtualização de rede Hyper-V e como elas diferem das VLANs (redes locais virtuais). Com a virtualização de rede Hyper-V, você cria sobreposição de redes virtuais, também chamadas de redes virtuais.



  
A SDN (rede definida pelo software) no Windows Server 2016 é baseada na política de programação para sobreposição de redes virtuais em um comutador virtual do Hyper-V. Você pode criar sobreposição de redes virtuais, também chamadas de redes virtuais, com virtualização de rede Hyper-V. 
  
Quando você implanta a virtualização de rede Hyper-V, as redes de sobreposição são criadas encapsulando o quadro de Ethernet de camada 2 da máquina virtual de locatário original com um cabeçalho de sobreposição ou de túnel (por exemplo, VXLAN ou NVGRE) e IP de camada 3 e Ethernet de camada 2 cabeçalhos da rede underlay (ou física). As redes virtuais de sobreposição são identificadas por um VNI (identificador de rede virtual) de 24 bits para manter o isolamento do tráfego de locatário e para permitir endereços IP sobrepostos. O VNI é composto por uma VSID (ID de sub-rede virtual), ID de comutador lógico e ID de túnel.  
  
Além disso, cada locatário recebe um domínio de roteamento (semelhante ao roteamento virtual e ao encaminhamento-VRF) para que vários prefixos de sub-rede virtual (cada um representado por um VNI) possam ser roteados diretamente entre si. Não há suporte para o roteamento entre locatários (ou domínio de roteamento cruzado) sem passar por um gateway.   
  
A rede física na qual o tráfego encapsulado de cada locatário é encapsulado é representada por uma rede lógica chamada rede lógica do provedor. Essa rede lógica do provedor consiste em uma ou mais sub-redes, cada uma representada por um prefixo de IP e, opcionalmente, uma marca de VLAN 802.1 q.  
  
Você pode criar redes lógicas e sub-redes adicionais para fins de infraestrutura para transportar o tráfego de gerenciamento, o tráfego de armazenamento, o tráfego de migração ao vivo, etc.  
  
O Microsoft SDN não oferece suporte ao isolamento de redes de locatário usando VLANs. O isolamento de locatário é realizado unicamente usando a sobreposição de redes virtuais e encapsulamento de virtualização de rede Hyper-V. 


