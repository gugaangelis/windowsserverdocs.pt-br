---
title: Visão geral do cenário de Test Lab DirectAccess Cluster-NLB
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess em um cluster com o NLB do Windows para Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a4e575c6c977fa429e650e001a05d0c90cb48f59
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951061"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>Visão geral do cenário de Test Lab DirectAccess Cluster-NLB

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Nesse cenário de laboratório de teste, o DirectAccess é implantado com:

-   **DC1**-um servidor configurado como um controlador de domínio, um servidor DNS (sistema de nomes de domínio) e um servidor DHCP (protocolo de configuração dinâmica de hosts).

-   **EDGE1**-um servidor na rede interna que está configurado como o primeiro servidor de acesso remoto em um cluster de servidor de acesso remoto. Este servidor tem dois adaptadores de rede; um conectado à rede interna e o outro conectado à rede externa.

-   **EDGE2**-um servidor na rede interna configurada como o segundo servidor de acesso remoto em um cluster de servidor de acesso remoto. Este servidor tem dois adaptadores de rede; um conectado à rede interna e o outro conectado à rede externa.

-   **App1**-um servidor na rede interna configurada como um servidor Web e de arquivos e como uma autoridade de certificação raiz corporativa (CA)

-   **App2**-um computador na rede interna configurada como um servidor Web e de arquivos IPv4. Este computador é usado para realçar os recursos de NAT64/DNS64.

-   **INET1**-um servidor configurado como um servidor DNS e DHCP da Internet.

-   **NAT1**-um computador cliente configurado como um dispositivo NAT (conversor de endereços de rede) usando o compartilhamento de conexão com a Internet.

-   **CLIENT1**-um computador cliente configurado como um cliente do DirectAccess que será usado para testar a conectividade do DirectAccess ao se mover entre a rede interna, a Internet simulada e uma rede doméstica.

O laboratório de teste consiste em três sub-redes que simulam o seguinte:

-   Uma rede doméstica chamada HomeNet (192.168.137.0/24) conectada à Internet por um NAT.

-   A rede externa representada pela sub-rede da Internet (131.107.0.0/24).

-   Uma rede interna chamada corpnet (10.0.0.0/24; 2001: DB8:1::/64) separada da Internet pelo servidor de acesso remoto.

Os computadores em cada sub-rede se conectam usando um Hub ou comutador virtual ou físico, conforme mostrado na figura a seguir.

![Visão geral do laboratório de teste](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)



