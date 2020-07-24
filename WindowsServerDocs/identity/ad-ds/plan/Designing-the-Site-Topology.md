---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: Criar a topologia de site
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 23c80ffa3137f5609abdba9abea08ea62d305573
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963788"
---
# <a name="designing-the-site-topology"></a>Criar a topologia de site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma topologia de site de serviço de diretório é uma representação lógica de sua rede física. A criação de uma topologia de site para Active Directory Domain Services (AD DS) envolve o planejamento de posicionamento do controlador de domínio e a criação de sites, sub-redes, links de site e pontes de links de site para garantir o roteamento eficiente de tráfego de consulta e replicação.  
  
A criação de uma topologia de site ajuda você a rotear com eficiência as consultas de cliente e Active Directory tráfego de replicação. Uma topologia de site bem projetada ajuda sua organização a obter os seguintes benefícios:  
  
-   Minimize o custo da replicação de dados de Active Directory.  
  
-   Minimize os esforços administrativos necessários para manter a topologia do site.  
  
-   Agendar a replicação que permite que os locais com links de rede lentos ou discadas repliquem Active Directory dados fora do horário de pico.  
  
-   Otimize a capacidade dos computadores cliente de localizar os recursos mais próximos, como controladores de domínio e servidores de Sistema de Arquivos Distribuído (DFS). Isso ajuda a reduzir o tráfego de rede em links de WAN (rede de longa distância), melhorar os processos de logon e logoff e acelerar as operações de download de arquivos.  
  
Antes de começar a projetar sua topologia de site, você deve entender sua estrutura de rede física. Além disso, você deve primeiro criar seu Active Directory estrutura lógica, incluindo a hierarquia administrativa, o plano de floresta e o plano de domínio para cada floresta. Você também deve concluir seu design de infraestrutura de DNS (sistema de nomes de domínio) para AD DS. Para obter mais informações sobre como projetar a estrutura lógica do Active Directory e a infraestrutura do DNS, consulte [criando a estrutura lógica para o Windows Server 2008 AD DS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770806(v=ws.10)).  
  
Depois de concluir o design da topologia do site, você deve verificar se os controladores de domínio atendem aos requisitos de hardware do Windows Server 2008 Standard, Windows Server 2008 Enterprise e Windows Server 2008 Datacenter.  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Noções básicas sobre a topologia de site do Active Directory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Como coletar informações de rede](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Como planejar o posicionamento de controlador de domínio](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Como criar um projeto de site](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Como criar um design de link de site](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Como criar um design de ponte de link de site](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Encontrando recursos adicionais para o Windows Server 2008 Active Directory Design da topologia do site](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  
