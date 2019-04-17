---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: Projetando a topologia de Site
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3f16b5b941ef9c3bd8f4bf742d432afc1b3f559a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-site-topology"></a>Projetando a topologia de Site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma topologia de site do serviço de diretório é uma representação lógica de sua rede física. Projetando uma topologia de site para os serviços de domínio do Active Directory (AD DS) envolve o planejamento de posicionamento de controlador de domínio e Projetando sites, sub-redes, links de site e pontes para garantir o roteamento eficiente de tráfego de consulta e replicação.  
  
Projetando uma topologia ajuda você a rotear consultas de cliente e o tráfego de replicação do Active Directory de forma eficiente. Uma topologia bem projetada ajuda sua empresa a obter os seguintes benefícios:  
  
-   Minimize o custo de replicação de dados do Active Directory.  
  
-   Minimize os esforços administrativos que são necessárias para manter a topologia de site.  
  
-   Agende a replicação que permite locais com conexões de rede dial-up ou lento com replicar os dados do Active Directory horários de pico.  
  
-   Otimize a capacidade de computadores cliente para localizar os recursos mais próximos, como controladores de domínio e servidores de sistema de arquivos distribuído (DFS). Isso ajuda a reduzir o tráfego de rede lenta longa distância (WAN) links de rede, melhorar processos de logon e logoff e acelerar operações de download de arquivo.  
  
Antes de começar a projetar a topologia de site, você deve entender sua estrutura de rede física. Além disso, você deve primeiro criar a estrutura lógica do Active Directory, incluindo a hierarquia administrativa, plano da floresta e plano de domínios de cada floresta. Você também deve preencher seu design de infraestrutura de sistema de nome de domínio (DNS) para o AD DS. Para obter mais informações sobre o design de sua estrutura lógica do Active Directory e infraestrutura DNS, consulte [Projetando a lógica estrutura para o Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx).  
  
Depois de concluir o design da topologia de site, você deve verificar se os controladores de domínio atendem aos requisitos de hardware para Windows Server 2008 Standard Windows Server 2008 Enterprise e Windows Server 2008 Datacenter.  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Noções básicas sobre a topologia de sites do Active Directory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Coletando informações de rede](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Planejar o posicionamento do controlador de domínio](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Criando um projeto de Site](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Criando um projeto de Link de Site](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Criando um projeto de ponte de Link de Site](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Encontrar recursos adicionais para Design de topologia de Site do Windows Server 2008 Active Directory](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


