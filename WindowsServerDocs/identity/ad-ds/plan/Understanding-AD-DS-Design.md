---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: "Noções básicas sobre o Design do AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 09afe3d19add87327d05bfafba6e0492a278b968
ms.sourcegitcommit: 1c3e6375b2e8eb01cfd299d0e9478fee46905c99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2018
---
# <a name="understanding-ad-ds-design"></a>Noções básicas sobre o Design do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

As organizações podem usar os serviços de domínio do Active Directory (AD DS) no Windows Server para simplificar o gerenciamento de usuário e recursos durante a criação de infraestruturas escalonáveis, seguras e gerenciáveis. Você pode usar o AD DS para gerenciar sua infraestrutura de rede, incluindo filial, Microsoft Exchange Server e ambientes de várias florestas.  
  
Um projeto de implantação do AD DS envolve três fases: uma fase de design, uma fase de implantação e uma fase de operações. Durante a fase de design, a equipe de design cria um design para a estrutura lógica do AD DS que melhor atenda às necessidades de cada divisão da organização que usará o serviço de diretório. Depois que o design for aprovado, a equipe de implantação testa o design em um ambiente de laboratório e, em seguida, implementa o design no ambiente de produção. Como o teste é realizado pela equipe de implantação e ele pode afeta a fase de design, é uma atividade intermediária que se sobreponha o design e implantação. Quando a implantação for concluída, a equipe de operações é responsável por manter o serviço de diretório.  
  
Embora as estratégias de design e implantação do Windows Server AD DS que são apresentadas neste guia se baseiam no laboratório extensivo e os testes de programa piloto e a implementação bem-sucedida em ambientes de cliente, talvez você precise personalizar seu design do AD DS e implantação para adaptá-lo às ambientes específicos e complexos.  
  
-   Para obter mais informações sobre a implantação do AD DS no ambiente de filiais, consulte o Read-Only guia de planejamento do controlador de domínio (RODC) Branch Office ([https://go.microsoft.com/fwlink/?LinkId=100207](https://go.microsoft.com/fwlink/?LinkId=100207)).  
  
-   Para obter mais informações sobre a implantação do AD DS em um ambiente do Exchange, consulte Exchange 2007 - Planejando o Active Directory ([https://go.microsoft.com/fwlink/?LinkId=88904](https://go.microsoft.com/fwlink/?LinkId=88904)).  
  
-   Para obter mais informações sobre a implantação do AD DS em um ambiente de várias florestas, consulte Considerações sobre várias florestas no Windows 2000 e Windows Server 2003 ([https://go.microsoft.com/fwlink/?LinkId=88905](https://go.microsoft.com/fwlink/?LinkId=88905)).  
  


