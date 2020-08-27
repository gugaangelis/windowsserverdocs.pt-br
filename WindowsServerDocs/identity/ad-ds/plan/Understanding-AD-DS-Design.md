---
ms.assetid: d590c90e-9adf-4305-b226-eb2a5743337b
title: Noções básicas do design do AD DS
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: b457bd4ea2517fdebf024caceccbc1b19857f754
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938346"
---
# <a name="understanding-ad-ds-design"></a>Noções básicas do design do AD DS

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

As organizações podem usar o Active Directory Domain Services (AD DS) no Windows Server para simplificar o gerenciamento de usuários e recursos durante a criação de infraestruturas escalonáveis, seguras e gerenciáveis. Você pode usar AD DS para gerenciar sua infraestrutura de rede, incluindo filiais, Microsoft Exchange Server e vários ambientes de floresta.

Um projeto de implantação AD DS envolve três fases: uma fase de design, uma fase de implantação e uma fase de operações. Durante a fase de design, a equipe de design cria um design para a estrutura lógica AD DS que melhor atende às necessidades de cada divisão na organização que usará o serviço de diretório. Depois que o design é aprovado, a equipe de implantação testa o design em um ambiente de laboratório e, em seguida, implementa o design no ambiente de produção. Como o teste é realizado pela equipe de implantação e, potencialmente, afeta a fase de design, é uma atividade provisória que se sobrepõe ao design e à implantação. Quando a implantação for concluída, a equipe de operações será responsável por manter o serviço de diretório.

Embora as estratégias de design e implantação do Windows Server AD DS que são apresentadas neste guia se baseiam em testes extensivos de laboratório e de programa piloto e na implementação bem-sucedida em ambientes de clientes, talvez seja necessário personalizar o design e a implantação de AD DS para melhor atender a ambientes específicos e complexos.

- Para obter mais informações sobre a implantação de AD DS em um ambiente de filial, consulte o [Guia de planejamento de filiais do RODC (controlador de domínio somente leitura)](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd734758(v=ws.10)).
- Para obter mais informações sobre como implantar AD DS em um ambiente do Exchange, consulte o artigo [Active Directory em organizações do Exchange Server](/exchange/plan-and-deploy/active-directory/active-directory).
- Para obter mais informações sobre a implantação de AD DS em um ambiente de várias florestas, consulte o artigo [várias considerações sobre floresta no windows 2000 e no Windows Server 2003](/previous-versions/windows/it-pro/windows-server-2003/cc739395(v=ws.10)).
