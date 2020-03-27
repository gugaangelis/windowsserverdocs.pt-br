---
title: Rede definida pelo software (SDN)
description: A Rede Definida pelo Software (SDN) fornece um método para configurar e gerenciar centralmente dispositivos de rede física e virtual, como roteadores, comutadores e gateways no seu data center. Use este tópico para saber mais sobre as tecnologias de SDN (rede definida pelo software) fornecidas no Windows Server, no System Center e no Microsoft Azure.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: lizross
author: eross-msft
ms.date: 08/09/2018
ms.openlocfilehash: a1283c6afcebe7b6abc12f9847865d6305bd3f2c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317272"
---
# <a name="sdn-in-windows-server-overview"></a>SDN na visão geral do Windows Server

>Aplicável a: Windows Server (canal semestral), Windows Server 2016


A Rede Definida pelo Software (SDN) fornece um método para configurar e gerenciar centralmente dispositivos de rede física e virtual, como roteadores, comutadores e gateways no seu data center. Você pode usar seus dispositivos existentes compatíveis com SDN para obter uma integração mais profunda entre a rede virtual e a rede física. Elementos de rede virtual, como comutador virtual Hyper-v, virtualização de rede Hyper-x e gateway RAS, são projetados para serem elementos integrantes da sua infraestrutura de SDN. 

>[!Note]
>Hosts Hyper-V e máquinas virtuais (VMs) executando servidores de infraestrutura SDN, como controladores de rede e nós de balanceamento de carga de software, devem ter o Windows Server 2016 Datacenter Edition instalado. 
>
>Os hosts do Hyper-V que contêm apenas VMs de carga de trabalho de locatário conectadas a redes controladas por SDN podem usar o Windows Server 2016 Standard Edition.

O SDN é possível porque os planos de rede não estão mais ligados aos próprios dispositivos de rede. No entanto, outras entidades, como software de gerenciamento de datacenter, como o System Center 2016, usam planos de rede. O SDN permite que você gerencie sua rede de datacenter dinamicamente, fornecendo uma maneira automatizada e centralizada para atender aos requisitos de seus aplicativos e cargas de trabalho. 

Você pode usar o SDN para:

- Crie, proteja e conecte dinamicamente sua rede para atender às necessidades em evolução de seus aplicativos
- Acelere a implantação de suas cargas de trabalho de maneira sem interrupções
- Conter vulnerabilidades de segurança de disseminar pela rede
- Definir e controlar políticas que regem redes físicas e virtuais 
- Implemente políticas de rede consistentemente em escala

O SDN permite que você realize tudo isso, além de reduzir os custos gerais de infraestrutura.



## <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>Entre em contato com a equipe de produto do datacenter e de rede em nuvem

Se você estiver interessado em discutir as tecnologias de SDN com a Microsoft ou outros clientes do SDN, há uma variedade de métodos para fazer contato.

Para obter mais informações, consulte [entre em contato com a equipe de datacenter e de rede em nuvem](contact-sdn-team.md).
