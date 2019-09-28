---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: Criar a topologia de site
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d3ebc3bd764a8ed44e201d0fca5f85b06df8be9d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408895"
---
# <a name="designing-the-site-topology"></a>Criar a topologia de site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma topologia de site de serviço de diretório é uma representação lógica de sua rede física. A criação de uma topologia de site para Active Directory Domain Services (AD DS) envolve o planejamento de posicionamento do controlador de domínio e a criação de sites, sub-redes, links de site e pontes de links de site para garantir o roteamento eficiente de tráfego de consulta e replicação.  
  
A criação de uma topologia de site ajuda você a rotear com eficiência as consultas de cliente e Active Directory tráfego de replicação. Uma topologia de site bem projetada ajuda sua organização a obter os seguintes benefícios:  
  
-   Minimize o custo da replicação de dados de Active Directory.  
  
-   Minimize os esforços administrativos necessários para manter a topologia do site.  
  
-   Agendar a replicação que permite que os locais com links de rede lentos ou discadas repliquem Active Directory dados fora do horário de pico.  
  
-   Otimize a capacidade dos computadores cliente de localizar os recursos mais próximos, como controladores de domínio e servidores de Sistema de Arquivos Distribuído (DFS). Isso ajuda a reduzir o tráfego de rede em links de WAN (rede de longa distância), melhorar os processos de logon e logoff e acelerar as operações de download de arquivos.  
  
Antes de começar a projetar sua topologia de site, você deve entender sua estrutura de rede física. Além disso, você deve primeiro criar seu Active Directory estrutura lógica, incluindo a hierarquia administrativa, o plano de floresta e o plano de domínio para cada floresta. Você também deve concluir seu design de infraestrutura de DNS (sistema de nomes de domínio) para AD DS. Para obter mais informações sobre como projetar a estrutura lógica do Active Directory e a infraestrutura do DNS, consulte [criando a estrutura lógica para o Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx).  
  
Depois de concluir o design da topologia do site, você deve verificar se os controladores de domínio atendem aos requisitos de hardware do Windows Server 2008 Standard, Windows Server 2008 Enterprise e Windows Server 2008 Datacenter.  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Noções básicas sobre a topologia de site do Active Directory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Como coletar informações de rede](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Como planejar o posicionamento de controlador de domínio](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Como criar um projeto de site](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Como criar um design de link de site](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Como criar um design de ponte de link de site](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Encontrando recursos adicionais para o Windows Server 2008 Active Directory Design da topologia do site](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


