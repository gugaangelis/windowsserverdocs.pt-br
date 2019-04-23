---
title: Visão geral do cenário de Test Lab
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9afeced4-1a9b-4cb3-9fc4-d7e44c675755
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5943986620328049f89b4613bb415fefca8a764f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875267"
---
# <a name="overview-of-the-test-lab-scenario"></a>Visão geral do cenário de Test Lab

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Nesse cenário de laboratório de teste, o DirectAccess é implantado com:  
  
-   **DC1**- um servidor que está configurado como um controlador de domínio, servidor DNS e servidor DHCP para o domínio corp.contoso.com.  
  
-   **2-DC1**– um servidor que está configurado como um controlador de domínio e servidor DNS para o domínio corp2.corp.contoso.com.  
  
-   **EDGE1 e EDGE1 2**-dois servidores estão configurados como servidores de acesso remoto na rede interna. Cada servidor tem dois adaptadores de rede; um conectado à rede interna e o outro conectado à rede externa.  
  
-   **App1 e 2-APP1**-dois servidores estão configurados como servidores web e de arquivos na rede interna.  
  
-   **APP2**- um computador na rede interna que é configurado como um servidor web e de arquivos da somente IPv4. Este computador é usado para realçar os recursos do NAT64 e o DNS64.  
  
-   **Roteador 1**- um servidor que está configurado para fornecer roteamento entre as duas redes corporativas internas.  
  
-   **INET1**– um servidor que está configurado como um servidor DHCP e DNS da Internet.  
  
-   **NAT1**- um computador cliente que está configurado como um dispositivo NAT (conversor) do endereço de rede usando o compartilhamento de Conexão com a Internet.  
  
-   **CLIENT1 e CLIENT2**-dois computadores cliente que são configurados como clientes DirectAccess que serão usados para testar a conectividade do DirectAccess durante a movimentação entre a rede interna, a Internet simulada e uma rede doméstica. **CLIENT2** é o Windows 7&reg; cliente.  
  
O laboratório de teste consiste em quatro sub-redes que simulam o seguinte:  
  
-   Uma rede doméstica chamada da rede doméstica (192.168.137.0/24) conectado à Internet por um NAT.  
  
-   A rede externa representada por sub-rede da Internet (131.107.0.0/24).  
  
-   Uma rede interna chamada Corpnet (10.0.0.0/24; 2001:db8:1::/ 64) separada da Internet pelo servidor de acesso remoto de EDGE1.  
  
-   Uma rede interna denominada Corpnet1 2 (10.2.0.0/24; 2001:db8:2:::/ 64) separada da Internet pelo servidor de acesso remoto EDGE1 2.  
  
Computadores em cada sub-rede se conectam usando um hub físico ou virtual ou o comutador, conforme mostrado na figura a seguir.  
  
![Visão geral do laboratório de teste](../../../media/Overview-of-the-Test-Lab-Scenario_4/TLG_DA_Multisite.png)  
  


