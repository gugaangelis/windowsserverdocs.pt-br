---
title: Visão geral do cenário de Test Lab DirectAccess Cluster-NLB
description: Este tópico faz parte do guia de laboratório de teste - demonstração do DirectAccess em um Cluster com Windows NLB para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 118a07b761979d3c32a8f7cf4f149aed56a1a893
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863577"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>Visão geral do cenário de Test Lab DirectAccess Cluster-NLB

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Nesse cenário de laboratório de teste, o DirectAccess é implantado com:  
  
-   **DC1**- um servidor que está configurado como um controlador de domínio, servidor de sistema de nome de domínio (DNS) e o servidor de protocolo de configuração de Host dinâmico (DHCP).  
  
-   **EDGE1**– um servidor na rede interna que é configurado como o primeiro servidor de acesso remoto em um cluster de servidor de acesso remoto. Esse servidor tiver dois adaptadores de rede; um conectado à rede interna e o outro conectado à rede externa.  
  
-   **EDGE2**– um servidor na rede interna que é configurado como o segundo servidor de acesso remoto em um cluster de servidor de acesso remoto. Esse servidor tiver dois adaptadores de rede; um conectado à rede interna e o outro conectado à rede externa.  
  
-   **App1**– um servidor na rede interna que é configurado como um servidor web e de arquivos e como uma autoridade de certificação raiz corporativa (CA)  
  
-   **APP2**- um computador na rede interna que é configurado como um servidor web e de arquivos da somente IPv4. Este computador é usado para realçar os recursos do NAT64 e o DNS64.  
  
-   **INET1**– um servidor que está configurado como um servidor DHCP e DNS da Internet.  
  
-   **NAT1**- um computador cliente que está configurado como um dispositivo NAT (conversor) do endereço de rede usando o compartilhamento de Conexão com a Internet.  
  
-   **CLIENT1**- um computador cliente que está configurado como um cliente DirectAccess que será usado para testar a conectividade do DirectAccess durante a movimentação entre a rede interna, a Internet simulada e uma rede doméstica.  
  
O laboratório de teste consiste em três sub-redes que simulam o seguinte:  
  
-   Uma rede doméstica chamada da rede doméstica (192.168.137.0/24) conectado à Internet por um NAT.  
  
-   A rede externa representada por sub-rede da Internet (131.107.0.0/24).  
  
-   Uma rede interna chamada Corpnet (10.0.0.0/24; 2001:db8:1::/ 64) separada da Internet pelo servidor de acesso remoto.  
  
Computadores em cada sub-rede se conectam usando um hub físico ou virtual ou o comutador, conforme mostrado na figura a seguir.  
  
![Visão geral do laboratório de teste](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)  
  


