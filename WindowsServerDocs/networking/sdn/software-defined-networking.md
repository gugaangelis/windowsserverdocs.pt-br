---
title: Rede definida pelo software (SDN)
description: A Rede Definida pelo Software (SDN) fornece um método para configurar e gerenciar centralmente dispositivos de rede física e virtual, como roteadores, comutadores e gateways no seu data center. Use este tópico para saber mais sobre as tecnologias de Software Defined Networking (SDN) que são fornecidas no Windows Server, o System Center e o Microsoft Azure.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: pashort
author: shortpatti
ms.date: 08/09/2018
ms.openlocfilehash: a6c4db97b1c55d5114eba09251685ef0f2b840bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855227"
---
# <a name="sdn-in-windows-server-overview"></a>SDN na visão geral do Windows Server

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016


A Rede Definida pelo Software (SDN) fornece um método para configurar e gerenciar centralmente dispositivos de rede física e virtual, como roteadores, comutadores e gateways no seu data center. Você pode usar os dispositivos existentes do SDN compatível para obter uma integração mais profunda entre a rede virtual e a rede física. Elementos de rede virtual, como o comutador Virtual Hyper-V, virtualização de rede do Hyper-V e Gateway de RAS são projetados para serem elementos integrantes de sua infraestrutura SDN. 

>[!Note]
>Hosts Hyper-V e máquinas virtuais (VMs) executando servidores de infraestrutura SDN, como nós de controlador de rede e balanceamento de carga de Software, deve ter o Windows Server 2016 Datacenter edition instalado. 
>
>Hosts do Hyper-V que contém somente locatário carga de trabalho VMs conectada às redes controlado de SDN podem usar o Windows Server 2016 Standard edition.

SDN é possível porque os planos de rede não estão mais restritos aos dispositivos de rede em si. No entanto, outras entidades, como software de gerenciamento de datacenter, como o System Center 2016 usam planos de rede. SDN permite que você gerencie a sua rede de datacenter dinamicamente, fornecendo uma maneira automatizada e centralizada de atender aos requisitos de seus aplicativos e cargas de trabalho. 

Você pode usar a SDN para:

- Criar, proteger e conectar sua rede para atender às necessidades dinâmicas dos seus aplicativos dinamicamente
- Acelerar a implantação das cargas de trabalho de uma maneira sem interrupções
- Conter vulnerabilidades de segurança se espalhe pela rede
- Definir e controlar as políticas que governam as redes físicas e virtuais 
- Implementar políticas de rede consistentemente em escala

SDN permite fazer tudo isso enquanto também reduz os custos de infraestrutura geral.



## <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>Entre em contato com a equipe de produto do Datacenter e a rede de nuvem

Se você estiver interessado em discutindo tecnologias SDN com a Microsoft ou outros clientes SDN, há uma variedade de métodos para a tomada de contato.

Para obter mais informações, consulte [entre em contato com a equipe de rede de nuvem e Datacenter](contact-sdn-team.md).
