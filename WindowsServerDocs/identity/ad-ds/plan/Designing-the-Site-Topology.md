---
ms.assetid: eeb919de-e21e-48d8-8186-e42adec6933f
title: Criar a topologia de site
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e1d9323ceda478369973f959687d46c9ca3cb88f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888487"
---
# <a name="designing-the-site-topology"></a>Criar a topologia de site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma topologia de site do serviço de diretório é uma representação lógica de sua rede física. Criando uma topologia de site para os serviços de domínio do Active Directory (AD DS) envolve o planejamento de posicionamento de controlador de domínio e criar sites, sub-redes, links de site e pontes de link de site para garantir o roteamento eficiente de tráfego de replicação e consulta.  
  
Criando uma topologia de site ajuda a rotear de forma eficiente a consultas de cliente e o tráfego de replicação do Active Directory. Uma topologia de site bem projetado ajuda sua organização alcançar os seguintes benefícios:  
  
-   Minimize o custo de replicação de dados do Active Directory.  
  
-   Minimize os esforços administrativos que são necessárias para manter a topologia de site.  
  
-   Agende a replicação que permite que os locais com links de rede lenta ou discada para replicar dados do Active Directory durante o horário de pico.  
  
-   Otimize a capacidade de computadores cliente para localizar os recursos mais próximos, como controladores de domínio e servidores de sistema de arquivos distribuído (DFS). Isso ajuda a reduzir o tráfego de rede pela conexão lenta links de (longa distância WAN) de rede, melhorar os processos de logon e logoff e acelerar as operações de download de arquivo.  
  
Antes de começar a projetar a topologia de site, você deve entender a estrutura da rede física. Além disso, você primeiro deve projetar a estrutura lógica do Active Directory, incluindo a hierarquia administrativa, o plano da floresta e o plano de domínio para cada floresta. Você também deve concluir o design de infraestrutura do sistema de nome de domínio (DNS) para o AD DS. Para obter mais informações sobre a criação de sua estrutura lógica do Active Directory e a infraestrutura DNS, consulte [Projetando a lógica estrutura para o Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx).  
  
Depois de concluir o design da topologia de site, você deve verificar se os controladores de domínio aos requisitos de hardware para Windows Server 2008 Standard, Windows Server 2008 Enterprise e Datacenter do Windows Server 2008.  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Noções básicas sobre a topologia de Site do Active Directory](../../ad-ds/plan/Understanding-Active-Directory-Site-Topology.md)  
  
-   [Coletando informações de rede](../../ad-ds/plan/Collecting-Network-Information.md)  
  
-   [Planejar o posicionamento de controlador de domínio](../../ad-ds/plan/Planning-Domain-Controller-Placement.md)  
  
-   [Criando um projeto de Site](../../ad-ds/plan/Creating-a-Site-Design.md)  
  
-   [Criar um Design de Link de Site](../../ad-ds/plan/Creating-a-Site-Link-Design.md)  
  
-   [Criar um Design de ponte de Link de Site](../../ad-ds/plan/Creating-a-Site-Link-Bridge-Design.md)  
  
-   [Localizar recursos adicionais para o Design de topologia de Site do Windows Server 2008 Active Directory](../../ad-ds/plan/Finding-Additional-Resources-for-Windows-Server-2008-Active-Directory-Site-Topology-Design.md)  
  


