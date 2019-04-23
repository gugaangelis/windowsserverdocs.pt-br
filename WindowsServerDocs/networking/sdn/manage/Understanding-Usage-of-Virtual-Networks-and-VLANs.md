---
title: Entender o uso de redes virtuais e VLANs
description: Neste tópico, você aprenderá sobre redes virtuais de virtualização de rede do Hyper-V e como eles diferem de redes locais virtuais (VLANs). Com a virtualização de rede do Hyper-V, você deve criar redes virtuais de sobreposição, também chamados de redes virtuais.
manager: dougkim
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
ms.date: 08/26/2018
ms.openlocfilehash: d126e97a91e4c61ecff00cc2b5a527618b2d4d0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875527"
---
# <a name="understand-the-usage-of-virtual-networks-and-vlans"></a>Entender o uso de redes virtuais e VLANs

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você aprenderá sobre redes virtuais de virtualização de rede do Hyper-V e como eles diferem de redes locais virtuais (VLANs). Com a virtualização de rede do Hyper-V, você deve criar redes virtuais de sobreposição, também chamados de redes virtuais.



  
Software Defined Networking (SDN) no Windows Server 2016 é com base em política para redes virtuais dentro de um comutador Virtual do Hyper-V de sobreposição de programação. Você pode criar redes virtuais de sobreposição, também chamados de redes virtuais, com a virtualização de rede do Hyper-V. 
  
Quando você implanta a virtualização de rede do Hyper-V, redes de sobreposição são criados, encapsulando o quadro de Ethernet de camada 2 o locatário da máquina virtual original com um cabeçalho de sobreposição - ou túnel - (por exemplo, VXLAN ou NVGRE) e IP de camada 3 e Ethernet de camada 2 rede de cabeçalhos de base (ou físico). As redes virtuais de sobreposição são identificadas por um 24 bits Virtual rede identificador (VNI) para manter o isolamento do tráfego de locatário e para permitir endereços IP sobrepostos. O VNI é composto de uma sub-rede virtual VSID (ID), a ID do comutador lógico e a ID do túnel.  
  
Além disso, cada locatário é atribuído a um domínio de roteamento (semelhante ao roteamento virtual e o encaminhamento - VRF) para que vários prefixos de sub-rede virtual (cada um representado por um VNI) podem ser roteados diretamente entre si. Entre locatários (ou entre domínios de roteamento) não há suporte para roteamento sem passar por um gateway.   
  
A rede física na qual o tráfego encapsulado de cada locatário é encapsulado é representada por uma rede lógica chamada de rede lógica do provedor. Esta rede lógica do provedor consiste em uma ou mais sub-redes, cada um representado por um prefixo de IP e, opcionalmente, uma VLAN 802.1q marca.  
  
Você pode criar redes lógicas adicionais e sub-redes para fins de infraestrutura transportar o tráfego de gerenciamento, o tráfego de armazenamento, ao vivo o tráfego da migração, etc.  
  
Microsoft SDN não oferece suporte para o isolamento de redes de locatário usando VLANs. Isolamento de locatários é realizado exclusivamente por meio de redes virtuais de sobreposição de virtualização de rede do Hyper-V e encapsulamento. 


