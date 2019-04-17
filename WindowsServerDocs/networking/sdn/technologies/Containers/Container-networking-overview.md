---
title: Visão geral de rede de contêiner
description: Este tópico é uma visão geral da pilha de rede para contêineres do Windows e inclui links para orientações adicionais sobre como criar, configurar e gerenciar redes de contêiner.
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: pashort
author: jmesser81
ms.openlocfilehash: fd2f022948208d4aacce2994ff053e77384b28fc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="container-networking-overview"></a>Visão geral de rede de contêiner

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico é uma visão geral da pilha de rede para contêineres do Windows e inclui links para orientações adicionais sobre como criar, configurar e gerenciar redes de contêiner.

Windows Server contêineres são um método de virtualização de sistema operacional leve usado para separar os serviços ou aplicativos de outros serviços que são executados no mesmo contêiner host. Para habilitar esse recurso, cada contêiner tem seu próprio modo de exibição do sistema operacional, processos, sistema de arquivos, do registro e endereços IP.

Função de contêineres do Windows da mesma forma para máquinas virtuais em relação à rede. Cada contêiner tem um adaptador de rede virtual que está conectado a um comutador virtual, por meio da qual tráfego de entrada e saído é encaminhado. Para impor o isolamento entre contêineres no mesmo host, um compartimento de rede é criado para cada Windows Server e Hyper-V contêiner no qual o adaptador de rede para o contêiner está instalado. Contêineres do Windows Server usam um vNIC Host para anexar ao switch virtual. Contêineres do Hyper-V use uma NIC VM sintéticos (não exposta a VM utilitário) para anexar ao switch virtual. 

Pontos de extremidade do contêiner podem ser anexados a uma rede de host local (por exemplo, NAT), a rede física ou uma rede virtual sobreposição criada através da pilha do Microsoft Software definido de rede (SDN). 

Para obter mais informações sobre como criar e gerenciar redes de contêiner para implantações de não-sobreposição/SDN, consulte o [Windows contêiner Networking](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking) guia no MSDN.

Para obter mais informações sobre como criar e gerenciar redes de contêiner para redes virtuais sobreposição com SDN, consulte [se conectar a pontos de extremidade do contêiner a uma rede virtual locatário ](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md). 