---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Noções básicas do design do AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d69229557af148ed82e0c2fac754d6b812e52e2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821769"
---
# <a name="understanding-ad-ds-design"></a>Noções básicas do design do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

As organizações podem usar o Active Directory Domain Services (AD DS) no Windows Server para simplificar o gerenciamento de usuários e recursos durante a criação de infraestruturas escalonáveis, seguras e gerenciáveis. Você pode usar AD DS para gerenciar sua infraestrutura de rede, incluindo filiais, Microsoft Exchange Server e vários ambientes de floresta.  
  
Um projeto de implantação AD DS envolve três fases: uma fase de design, uma fase de implantação e uma fase de operações. Durante a fase de design, a equipe de design cria um design para a estrutura lógica AD DS que melhor atende às necessidades de cada divisão na organização que usará o serviço de diretório. Depois que o design é aprovado, a equipe de implantação testa o design em um ambiente de laboratório e, em seguida, implementa o design no ambiente de produção. Como o teste é realizado pela equipe de implantação e, potencialmente, afeta a fase de design, é uma atividade provisória que se sobrepõe ao design e à implantação. Quando a implantação for concluída, a equipe de operações será responsável por manter o serviço de diretório.  
  
Embora as estratégias de design e implantação do Windows Server AD DS que são apresentadas neste guia se baseiam em testes extensivos de laboratório e de programa piloto e na implementação bem-sucedida em ambientes de clientes, talvez seja necessário personalizar o design e a implantação de AD DS para melhor atender a ambientes específicos e complexos.
  
- Para obter mais informações sobre a implantação de AD DS em um ambiente de filial, consulte o [Guia de planejamento de filiais do RODC (controlador de domínio somente leitura)](https://go.microsoft.com/fwlink/?LinkId=100207).  
- Para obter mais informações sobre a implantação de AD DS em um ambiente do Exchange, consulte o artigo [Exchange 2007-Planning Active Directory](https://go.microsoft.com/fwlink/?LinkId=88904).  
- Para obter mais informações sobre a implantação de AD DS em um ambiente de várias florestas, consulte o artigo [várias considerações sobre floresta no windows 2000 e no Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=88905).  
