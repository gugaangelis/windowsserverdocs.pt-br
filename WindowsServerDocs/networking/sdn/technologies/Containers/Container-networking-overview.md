---
title: Visão geral de rede de contêineres
description: Este tópico é uma visão geral da pilha da rede para contêineres do Windows e inclui links para orientações adicionais sobre como criar, configurar e gerenciar as redes de contêiner.
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
ms.date: 09/04/2018
ms.openlocfilehash: 72b1ac739d9012ac7b90e97abe22e5f321ddba63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886137"
---
# <a name="container-networking-overview"></a>Visão geral de rede de contêineres

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, nós fornecemos a você uma visão geral da pilha da rede para contêineres do Windows e incluímos links para orientações adicionais sobre como criar, configurar e gerenciar as redes de contêiner.

Contêineres do Windows Server são um método de virtualização do sistema operacional leve separar aplicativos ou serviços de outros serviços que são executados no mesmo host do contêiner. Função de contêineres do Windows da mesma forma que as máquinas virtuais. Quando habilitado, cada contêiner tem uma exibição separada do sistema operacional, processos, sistema de arquivos, registro e endereços IP, que você pode se conectar às redes virtuais. 

Um contêiner do Windows compartilha um kernel com o host do contêiner e todos os contêineres em execução no host. Devido ao espaço de kernel compartilhado, esses contêineres exigem a mesma versão e configuração de kernel. Os contêineres fornecem isolamento de aplicativo por meio da tecnologia de isolamento de processo e de namespace.

>[!IMPORTANT]
>Contêineres do Windows não fornecem um limite de segurança hostil e não devem ser usados para isolar o código não confiável. 

Com os contêineres do Windows, você pode implantar um host Hyper-V, em que você cria uma ou mais máquinas virtuais nos hosts de VM. Dentro dos hosts VM, contêineres são criados e o acesso de rede é por meio de um comutador virtual em execução dentro da máquina virtual. Você pode usar imagens reutilizáveis armazenadas em um repositório para implantar o sistema operacional e serviços em contêineres. Cada contêiner tem um adaptador de rede virtual que se conecta a um comutador virtual, encaminhar o tráfego de entrada e saído. Você pode anexar os pontos de extremidade do contêiner para uma rede de host local (por exemplo, o NAT), a rede física ou rede virtual de sobreposição criado por meio da pilha SDN.

Para impor o isolamento entre contêineres no mesmo host, você deve criar um compartimento de rede para cada contêiner do Windows Server e Hyper-V. Contêineres do Windows Server usam uma vNIC do host para se conectar ao comutador virtual. Os contêineres do Hyper-V usam uma NIC de VM sintética (não exposta à VM do utilitário) para se conectar ao comutador virtual. 

## <a name="related-topics"></a>Tópicos relacionados 

- [Rede de contêiner do Windows](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture): Saiba como criar e gerenciar redes de contêiner para implantações de não-sobreposição/SDN.

- [Conectar pontos de extremidade de contêiner a uma rede virtual do locatário](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md): Saiba como criar e gerenciar redes de contêiner para redes virtuais de sobreposição com SDN. 