---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Noções básicas do design do AD DS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c94f6ddd19e3178243545b0cc71f6f4c7bb4dbec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828787"
---
# <a name="understanding-ad-ds-design"></a>Noções básicas do design do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

As organizações podem usar os serviços de domínio do Active Directory (AD DS) no Windows Server para simplificar o gerenciamento de usuário e recurso durante a criação de infraestruturas escalonáveis, seguras e gerenciáveis. Você pode usar o AD DS para gerenciar sua infraestrutura de rede, inclusive o escritório da filial, Microsoft Exchange Server e ambientes de várias florestas.  
  
Um projeto de implantação do AD DS envolve três fases: uma fase de design, uma fase de implantação e uma fase de operações. Durante a fase de design, a equipe de design cria um projeto para a estrutura lógica do AD DS que melhor atenda às necessidades de cada divisão da organização que usará o serviço de diretório. Depois que o design for aprovado, a equipe de implantação testa o design em um ambiente de laboratório e, em seguida, implementa o design no ambiente de produção. Como o teste é executado pela equipe de implantação e potencialmente afeta a fase de design, é uma atividade intermediária que se sobrepõe a design e implantação. Quando a implantação for concluída, a equipe de operações é responsável por manter o serviço de diretório.  
  
Embora as estratégias de design e implantação do Windows Server AD DS que são apresentadas neste guia baseiam-se no laboratório extensivo e programa piloto de teste e implementação com êxito em ambientes de cliente, talvez você precise personalizar o design do AD DS e implantação para atender melhor às ambientes específicos e complexos.
  
- Para obter mais informações sobre como implantar o AD DS em um ambiente da filial, consulte o [guia de planejamento do controlador de domínio somente leitura (RODC) Branch Office](https://go.microsoft.com/fwlink/?LinkId=100207).  
- Para obter mais informações sobre como implantar o AD DS em um ambiente do Exchange, consulte o artigo [Exchange 2007 - Planejando o Active Directory](https://go.microsoft.com/fwlink/?LinkId=88904).  
- Para obter mais informações sobre como implantar o AD DS em um ambiente de várias florestas, consulte o artigo [considerações sobre várias florestas no Windows 2000 e Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=88905).  
