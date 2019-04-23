---
title: Configurar redes locais virtuais do Hyper-V
description: Fornece instruções para configurar uma rede de área local virtual (VLAN) para uso pelas máquinas virtuais em um host Hyper-V.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
author: KBDAzure
ms.author: kathydav
ms.date: 10/11/2016
ms.openlocfilehash: 5b5eaf175e7c09124aaa3f7a33523e8b87a9ae84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848457"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>Configurar redes locais virtuais do Hyper-V
Redes locais virtuais \(VLANs\) oferecem uma maneira de isolar o tráfego de rede. As VLANs são configuradas em comutadores e roteadores que oferecem suporte a 802.1q. Se você configurar várias VLANs e deseja comunicação ocorra entre eles, você precisará configurar para permitir que os dispositivos de rede. 

Você precisará do seguinte para configurar VLANs:  
  
-   Um adaptador de rede física e um driver que dá suporte a 802.1q marcação de VLAN.  
-   Um comutador de rede física que dá suporte a 802.1q marcação de VLAN.  
  
No host, você vai configurar o comutador virtual para permitir o tráfego de rede na porta de comutador físico. Isso é para as IDs de VLAN que você deseja usar internamente com máquinas virtuais. Em seguida, você pode configurar a máquina virtual para especificar a VLAN em que a máquina virtual usará para todas as comunicações de rede.  
  
#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>Para permitir que um comutador virtual para usar uma VLAN  
  
1.  Abrir Hyper\-Manager V.  
  
2.  No menu Ações, clique em **Gerenciador de comutador Virtual**.  
  
3.  Sob **comutadores virtuais**, selecione um comutador virtual conectado a um adaptador de rede física que dá suporte a VLANs. 

4. No painel direito, a ID de VLAN, selecione **habilitar identificação de LAN virtual** e, em seguida, digite um número para o ID. de VLAN  
  
    Todo o tráfego que passa pelo adaptador de rede física conectada ao comutador virtual será marcado com a ID de VLAN que você definir.  
  
#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>Para permitir que uma máquina virtual para usar uma VLAN  
  
1.  Abrir Hyper\-Manager V.  
  
2.  No painel de resultados, sob **máquinas virtuais**, selecione a máquina virtual apropriada e, em seguida, clique com botão direito **configurações**.  

3.  Sob **Hardware**, selecione um comutador virtual que está configurado com uma VLAN.
  
4.  No painel direito, selecione **habilitar identificação de LAN virtual**, e, em seguida, digite a mesma ID de VLAN especificada para o comutador virtual. 

Se a máquina virtual precisar usar mais VLANs, siga um destes procedimentos:  
  
-   Conecte-se mais adaptadores de rede virtual para apropriado comutadores virtuais e atribuir IDs de VLAN. Certifique-se de configurar os endereços IP corretamente e que o tráfego que você deseja rotear através da VLAN também usa o endereço IP correto.  
  
-   Configurar o adaptador de rede virtual word no modo de tronco usando o [definir\-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx) cmdlt.
  
## <a name="see-also"></a>Consulte também  
 
[Hyper\-V Virtual Switch](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/hyper-v-virtual-switch)